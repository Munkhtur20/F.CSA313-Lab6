# A. Category-Partition — POST /transfer

## A1. Parameters and categories

| Parameter | Category | Representative values |
|---|---|---|
| From | Source account status | Valid, Frozen, Missing |
| To | Recipient account | Valid, Missing, SameAsFrom |
| Amount | Transfer amount | Normal, Zero, OverBalance, OverLimit |
| Currency | Currency type | Same, Different |

## A2. Constraints and test-design decisions

1. From = Missing үед Amount = Normal байна.
2. To = Missing үед Amount = Normal байна.
3. Different currency үед валют хөрвүүлсний дараах дүнгээр үлдэгдэл болон өдрийн лимитийг шалгана.
4. OverBalance утгыг шалгахдаа үлдэгдэл хүрэлцэхгүй боловч өдрийн лимит хүрэлцээтэй байхаар тохируулна.
5. OverLimit утгыг шалгахдаа үлдэгдэл хүрэлцээтэй боловч өдрийн лимит хүрэлцэхгүй байхаар тохируулна.
6. Environment буюу тестийн орчны далд хүчин зүйлс: дансны үлдэгдэл, өдрийн лимитийн үлдэгдэл, валютын ханш. Эдгээрийг PICT-ийн үндсэн дөрвөн параметрээс тусад нь тестийн өгөгдлөөр тохируулна.

### Error precedence assumption

Системийн шаардлагад алдааны дараалал тодорхой заагаагүй тул тестийн загварт дараах дарааллыг таамаглан тогтоов:

1. ERROR_NO_FROM
2. ERROR_FROZEN
3. ERROR_NO_TO
4. ERROR_SAME_ACCOUNT
5. ERROR_BAD_AMOUNT
6. ERROR_INSUFFICIENT
7. ERROR_LIMIT

Энэ дараалал нь тестийн загварчлалын таамаглал бөгөөд бодит системийн шаардлагаар баталгаажуулах хэрэгтэй.

## A3. Test specification

| ID | From | To | Amount | Currency | Expected result |
|---|---|---|---|---|---|
| TC01 | Valid | Valid | Normal | Same | 201 Created |
| TC02 | Missing | Valid | Normal | Same | ERROR_NO_FROM |
| TC03 | Frozen | Valid | Normal | Same | ERROR_FROZEN |
| TC04 | Valid | Missing | Normal | Same | ERROR_NO_TO |
| TC05 | Valid | SameAsFrom | Normal | Same | ERROR_SAME_ACCOUNT |
| TC06 | Valid | Valid | Zero | Same | ERROR_BAD_AMOUNT |
| TC07 | Valid | Valid | OverBalance | Same | ERROR_INSUFFICIENT |
| TC08 | Valid | Valid | OverLimit | Same | ERROR_LIMIT |
| TC09 | Valid | Valid | Normal | Different | 201 Created after conversion |
| TC10 | Missing | Missing | Normal | Same | ERROR_NO_FROM |
| TC11 | Frozen | Valid | Zero | Same | ERROR_FROZEN |
| TC12 | Valid | Valid | Normal | Same | 201 Created; SINGLE happy path |

For TC07, balance must be lower than the transfer amount while the daily limit is sufficient.
For TC08, balance must be sufficient while the remaining daily limit is lower than the transfer amount.
For TC09, the converted amount must be within both balance and daily limit.

## Combination calculation

Initial full combinations:

3 × 3 × 4 × 2 = 72

The IF constraints require Amount = Normal whenever From or To is Missing.

There are 5 From/To pairs affected by these constraints:
- From = Missing: 3 pairs
- To = Missing: 3 pairs
- Both Missing counted once: subtract 1

Therefore, 5 pairs allow only 1 Amount value, while the remaining 4 pairs allow 4 Amount values.

Valid combinations after constraints:

(5 × 1 + 4 × 4) × 2 = 42

Result: 72 → 42 valid combinations.

The PICT pairwise suite generated 13 rows without constraints and 12 rows with constraints.
These are test-suite row counts, not the total number of valid combinations.
