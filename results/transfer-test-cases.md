# POST /transfer — PICT Test Cases

## TC-01: Happy path
- From: Valid
- To: Valid
- Amount: Normal
- Currency: Same
- Setup: Хоёр данс идэвхтэй, үлдэгдэл болон өдрийн лимит хүрэлцээтэй.
- Input: Хэвийн дүнгээр ижил валютаар шилжүүлэг хийнэ.
- Expected: HTTP 201 Created, шилжүүлэг амжилттай.

## TC-02: Insufficient balance
- From: Valid
- To: Valid
- Amount: ~OverBalance
- Currency: Different
- Setup: Хөрвүүлсэн дүн дансны үлдэгдлээс их боловч өдрийн лимит хүрэлцээтэй.
- Input: Өөр валютаар үлдэгдлээс их дүн шилжүүлнэ.
- Expected: ERROR_INSUFFICIENT, шилжүүлэг хийгдэхгүй.

## TC-03: Missing source account
- From: ~Missing
- To: Valid
- Amount: Normal
- Currency: Same
- Setup: Илгээгчийн дансны ID системд бүртгэлгүй.
- Input: Байхгүй данснаас шилжүүлэг хийнэ.
- Expected: ERROR_NO_FROM.

## TC-04: Frozen source account
- From: ~Frozen
- To: Valid
- Amount: Normal
- Currency: Same
- Setup: Илгээгчийн данс царцаалттай.
- Input: Царцаалттай данснаас шилжүүлэг хийнэ.
- Expected: ERROR_FROZEN.
