# VHLDL KNOWLEDGE EXTRACTION SPECIFICATION v1.0 — CANDIDATE
## Đặc tả & Cẩm nang vận hành Knowledge Extraction cho Thư viện số Lịch sử & Văn học Việt Nam

**Ngày biên soạn Candidate:** 24.08.2026

Căn cứ biên soạn: MASTER PROMPT FOR VHLDL; KẾ HOẠCH DỰ ÁN VHLDL; các báo cáo kiểm kê/QA đã được chuyển trong dự án. Đây là Candidate, chưa Freeze.

## 0. TRẠNG THÁI TÀI LIỆU
- Trạng thái: CANDIDATE — chưa đóng băng.
- Mục đích: làm tài liệu nền để Project Office, Data/Extraction, QA/Regression, Prompt và các thành viên kế nhiệm có thể thực hiện công việc thống nhất.
- Nguyên tắc phiên bản: bản Candidate được phép tiếp tục hoàn thiện; chỉ sau khi Project Office và người phụ trách nghiệp vụ cùng duyệt mới phát hành FROZEN v1.0. Bản đã đóng băng không sửa trực tiếp; mọi thay đổi phải đi qua Change Request và phát hành phiên bản mới.
- Đây là đặc tả có khả năng mở rộng. Quy tắc nền tảng được kiểm soát chặt; các trường hợp mới, Entity mới, QA Rule mới hoặc cách xử lý mới có thể được đề xuất thông qua cơ chế OPEN/TBD/Change Request.

## 1. MỤC ĐÍCH VÀ PHẠM VI
- VHLDL Knowledge Extraction Specification định nghĩa cách biến nguồn PDF đã được xác nhận thành các Artifact có cấu trúc, có Traceability, có QA và có khả năng cung cấp cho Workbook/Index, Markdown, Knowledge Graph, Search, Hover và Website.
- Phạm vi Release 1.0 theo kế hoạch điều hành hiện hành: 15 tập Lịch sử Việt Nam và 42 tập Tổng tập Văn học Việt Nam; các chức năng mở rộng không thiết yếu được quản lý như Backlog/Release sau.
- Specification không thay thế Source Baseline, Data Contract, Website UI Baseline hoặc các quyết định của Project Office; nó quy định cách Extraction/Data thực hiện trong các ranh giới đó.

## 2. TRIẾT LÝ VẬN HÀNH
- Ưu tiên chất lượng dữ liệu hơn tốc độ: ĐÚNG – ĐỦ – KHÔNG TRÙNG LẶP – CÓ THỂ KIỂM CHỨNG.
- Source First / Evidence Before Conclusion / Traceability: mọi thông tin được đưa vào hệ thống phải có căn cứ từ nguồn hoặc được đánh dấu rõ là UNKNOWN/NEEDS REVIEW khi chưa đủ bằng chứng.
- Không được tạo thông tin mà nguồn không có. Không được dùng suy luận ngữ nghĩa để biến một khả năng thành dữ kiện xác định.
- Specification phải đủ chặt để bảo vệ dữ liệu nhưng đủ mở để phát triển. Không vì một trường hợp mới mà làm toàn bộ dự án dừng vô thời hạn.
- QA phát hiện, định vị, phân loại và ghi nhận lỗi; việc sửa theo quy trình phải giữ được lịch sử và Evidence. Trong vòng QA, không che lỗi bằng cách sửa trực tiếp Artifact mà chưa tạo dấu vết.

## 3. KIẾN TRÚC TỔNG THỂ
- Chuỗi nền: SOURCE → RETRIEVAL → EXTRACTION → NORMALIZED DATA → ENTITY → RELATION → INDEX → SEARCH → WEBSITE.
- Chuỗi kiểm soát: SOURCE → EXTRACTED → QA → TRACEABLE → INDEXED → LINKED → ACCEPTED.
- Website là lớp khai thác/presentation; không dùng Website để quyết định Data Model. Website thích ứng với Data Contract đã được xác lập.
- Knowledge Linking gồm các lớp Person, Author, Work, Event, Place, Dynasty, Literary Period và CrossReference; các liên kết phải có nguồn hoặc căn cứ được ghi nhận.

## 4. SOURCE BASELINE VÀ RETRIEVAL
- PDF nguồn đã được xác nhận là Source Baseline phải được giữ nguyên. Không sửa, thay thế hoặc “làm sạch” PDF nguồn để che lỗi OCR.
- Trước Extraction phải kiểm tra khả năng truy cập nguồn, cấu trúc tài liệu, trang, Printed Page/PDF Page khi có thể, và các vấn đề OCR/scan.
- Phân biệt rõ PDF Metadata, PDF Content, OCR, System Report, Manual Verification và UNKNOWN. System Report không mặc nhiên là Independent Verification.
- Nếu retrieval/page mapping/anchor chưa đủ bằng chứng, trạng thái phải là NEEDS REVIEW hoặc UNKNOWN; không tự chuyển PASS.

## 5. QUY TRÌNH EXTRACTION
- Bước 1: đọc toàn bộ tập, nhận diện chương, mục, phụ lục, chú thích, tài liệu tham khảo, bảng biểu và cấu trúc đặc biệt.
- Bước 2: trích xuất thực thể vào danh sách tạm; chưa ghi Workbook khi chưa qua kiểm tra cần thiết.
- Bước 3: kiểm tra trùng tên/trùng thực thể/trùng sự kiện; không xóa khi chưa chắc chắn, đánh dấu NEEDS REVIEW.
- Bước 4: ghi dữ liệu vào Workbook theo thứ tự Sheet đã được xác nhận.
- Bước 5: chỉ sau khi Sheet nền ổn định mới tạo Sheet công thức/tổng hợp.
- Bước 6: kiểm tra liên kết và các liên kết mồ côi.
- Output tối thiểu của một Golden Run: Markdown, Workbook, TOC, Search Index, Entities, Relationships, Anchor_ID, Relative_Link, Traceability, Cross-reference và QA Evidence.

## 6. WORKBOOK / INDEX
- Workbook là một cấu trúc dữ liệu trung tâm; phải có Specification/Baseline rõ ràng trước khi Mass Extraction.
- Thứ tự Sheet nền theo thiết kế hiện hành: 01_Person; 02_Event; 03_Location; 04_Work; 05_Book; 06_Reference; 07_Timeline; 08_Keyword; 09_Hover; 10_Search; 11_Relationship; 12_CrossLink; 13_Source; 14_Revision_Log.
- Các lớp công thức/tổng hợp như Knowledge Graph, Statistics, Dashboard, Index, Hover Database, Homepage Database, Search Engine và API Data chỉ lấy dữ liệu từ Sheet nền; không nhập tay nếu đã được xác định là Sheet dẫn xuất.
- Thứ tự Sheet là Baseline hiện hành trong tài liệu nguồn; nếu Workbook thực tế đã được Project Office khóa theo cấu trúc khác, phải ưu tiên Baseline mới nhất và ghi Change Log.

## 7. DATA DICTIONARY, ID VÀ DEPENDENCY MATRIX
- Mỗi trường dữ liệu quan trọng phải có tên, ý nghĩa, kiểu dữ liệu, trạng thái bắt buộc/tùy chọn, nguồn và quy tắc kiểm tra.
- ID phải ổn định và duy nhất trong phạm vi đã quy định. Không tự tạo ID mới nếu việc đó phá vỡ ID/Mapping đang khóa.
- Dependency Matrix phải mô tả Sheet/module được tạo khi nào, phụ thuộc Sheet nào, ảnh hưởng Sheet nào và khi sửa dữ liệu phải kiểm tra lại những phần nào.
- Dependency là cơ sở để xác định Critical Path và tránh sửa một điểm mà không biết tác động dây chuyền.

## 8. TRACEABILITY VÀ DATA CONTRACT
- Traceability phải cho phép đi từ dữ liệu → nguồn và, khi có thể, từ nguồn → dữ liệu.
- Các thành phần cần kiểm soát gồm Printed_Page, PDF_Page, Anchor_ID, Relative_Link, Volume, Chapter, Section, Source reference và Reverse Traceability khi có.
- Không coi một link có destination là đủ nếu actual Markdown link hoặc Evidence chưa tồn tại.
- Data Contract giữa Data và Website phải xác định trước khi Website dùng dữ liệu thật cho Search/Hover/Timeline/Entity. Website không tự thay đổi Data chỉ để phù hợp UI.

## 9. 05 VÒNG KIỂM TRA CHẤT LƯỢNG
- Vòng 1 — ĐÚNG: đối chiếu với sách; không suy diễn.
- Vòng 2 — ĐỦ: kiểm tra không bỏ sót nhân vật, sự kiện, địa danh, tác phẩm, tài liệu tham khảo, chú thích, phụ lục, bảng biểu, sơ đồ và các thành phần thuộc phạm vi.
- Vòng 3 — KHÔNG TRÙNG LẶP: kiểm tra tên, tên khác, bút danh, tên chữ, tên húy, miếu hiệu, niên hiệu, tên nước ngoài, Hán-Việt, Latin và các biến thể cần thiết.
- Vòng 4 — LIÊN KẾT: kiểm tra liên kết sai, thiếu, vòng, trùng và liên kết mồ côi.
- Vòng 5 — ĐỐI CHIẾU TOÀN BỘ: đối chiếu Workbook với sách/nguồn; thiếu thì bổ sung theo bằng chứng, sai thì ghi nhận/sửa theo quy trình, chưa chắc chắn thì NEEDS REVIEW.
- Mỗi vòng phải có Evidence và được ghi vào Data_Quality_Log; không được coi kiểm tra đã làm nếu không có dấu vết.

## 10. DATA QUALITY LOG
- Mỗi thực thể cần bản ghi kiểm tra phù hợp. Các trường tối thiểu: Entity_ID, Entity_Name, Sheet_Name, Check_01…Check_05, Sai lệch phát hiện, Nguyên nhân, Biện pháp hiệu chỉnh, Người/AI thực hiện, Ngày giờ kiểm tra, Trạng thái.
- Trạng thái tối thiểu: PASS, PASS WITH NOTE, NEEDS REVIEW, FAILED.
- Cột Sai lệch phát hiện phải ghi cụ thể: thiếu dữ liệu, trùng, sai chính tả, sai liên kết, sai niên đại, sai tên, sai nguồn, sai ID, sai Workbook, sai Markdown, sai Hover, sai Search, sai Homepage… khi có.
- Mọi hiệu chỉnh phải lưu lịch sử; không ghi đè làm mất lịch sử.
- Data_Quality_Log trong Documents và sheet tương ứng trong Workbook phải phản ánh cùng một hệ thống kiểm soát, với nguồn phiên bản rõ ràng.

## 11. ERROR TAXONOMY VÀ REGRESSION
- Những lỗi phát hiện từ LSVN_001 và TTVHVN_039 là nguồn thực nghiệm để xây Error Taxonomy và Regression Test.
- Nhóm lỗi cần kiểm soát theo kế hoạch: suy đoán; mất dấu; rời chữ; sai từ; UNKNOWN; Heading; chú thích; Page Mapping; Anchor; Traceability; Entity Extraction; Cross-reference; OCR corruption; số/năm; tên riêng; tài liệu tham khảo; đa ngôn ngữ; định dạng.
- Quy trình: Finding → Error Taxonomy → Pattern Detection → Root Cause → Prompt Requirement → Proposed Rule → Regression Test → Evidence → QA → Project Office Review.
- QA không tự sửa để làm cho kết quả đẹp hơn. Lỗi phải được giữ làm Evidence/Regression khi phù hợp.

## 12. MASTER PROMPT — NGUYÊN TẮC THIẾT KẾ
- Master Prompt là một thành phần thực thi của Specification, không phải toàn bộ Specification.
- Prompt phải bảo đảm: Source First; không suy đoán; bảo toàn nội dung; Traceability; cấu trúc Output; QA; trạng thái UNKNOWN/NEEDS REVIEW; Versioning.
- Prompt phải có vùng mở rộng: khi gặp trường hợp chưa quy định, không tự bịa quy tắc. Ghi OPEN/TBD/NEEDS REVIEW và tạo Change Request nếu cần.
- Không sửa Prompt trực tiếp giữa Mass Extraction. Lỗi mới → Change Request → đánh giá tác động → phiên bản mới → Regression. Việc Freeze chỉ có hiệu lực sau Golden Acceptance.
- Prompt phải có thể được triển khai bằng AI khác nhau mà vẫn giữ cùng tiêu chuẩn dữ liệu; khác biệt công cụ không được làm thay đổi nguyên tắc nền.

## 13. GOLDEN RUN, ACCEPTANCE VÀ FREEZE
- Golden Run trọng tâm: LSVN_001 và TTVHVN_039.
- Golden QA phải kiểm TOC, Printed Page, PDF Page, Anchor, Relative Link, Search Index, Entity, Relationship, Cross-reference và Reverse Traceability khi có.
- Golden Acceptance chỉ đạt khi Evidence đủ, Regression đạt, Traceability đạt và không còn lỗi Critical chưa xử lý.
- Sau Golden Acceptance mới Freeze Master Prompt và mở Mass Extraction.
- Controlled Pilot nên bao gồm ít nhất một tập Lịch sử, một tập Văn học, một tập cấu trúc phức tạp, một tập có chú thích/bảng và một trường hợp khó; đây là định hướng từ kế hoạch dự án.

## 14. MASS EXTRACTION THEO BATCH
- Không chạy 57 tập như một khối. Batch theo kế hoạch hiện hành: LSVN 5 + 5 + 5; TTVHVN 10 + 10 + 10 + 12.
- Sau mỗi Batch: Extraction → Structural QA → Spot Check → Accept/Reject.
- Nếu Batch phát hiện lỗi hệ thống, dừng Batch kế tiếp để đánh giá; không để lỗi lan sang toàn bộ 57 tập.
- Mỗi Volume chỉ được gọi DONE/ACCEPTED khi đạt Definition of Done.

## 15. CHANGE MANAGEMENT VÀ PHÁT TRIỂN MỞ
- Nguyên tắc nền tảng có thể được khóa; các Rule/Entity/Field/QA mới có thể mở rộng.
- Trường hợp mới: UNKNOWN/OPEN → ghi nhận → đánh giá → Change Request → xác định có cần thay đổi Prompt/Schema/Data Contract hay không → Regression → phát hành phiên bản.
- Version History phải ghi: phiên bản, ngày, thay đổi, lý do, tác động, người duyệt và các Regression Test liên quan.
- Không sửa trực tiếp bản FROZEN để tránh mất lịch sử.

## 16. PROJECT OFFICE VÀ ĐIỀU PHỐI
- Project Office là nơi tổng hợp trạng thái chung; MASTER_STATUS là Single Source of Truth.
- Nhật ký tối thiểu: MASTER_STATUS, PROJECT_SCHEDULE, DAILY_LOG, DECISION_LOG, CHANGE_LOG, RISK_REGISTER và DEPENDENCY_REGISTER.
- Cơ chế điều hành: Giao việc → Thực hiện → Báo cáo → Kiểm tra → Đánh giá → Điều chỉnh → Nghiệm thu → Cập nhật.
- Mỗi nhiệm vụ phải có OWNER, TASK, INPUT, OUTPUT, DEADLINE, ACCEPTANCE, DEPENDENCY và NEXT ACTION IF OVERDUE.
- Không có WAITING vô hạn. WAITING phải có Dependency + Owner + Deadline + Next Action + Escalation.

## 17. DEFINITION OF DONE
- Một Volume không được coi là DONE chỉ vì đã có Markdown.
- Chuỗi tối thiểu: SOURCE → EXTRACTED → QA → TRACEABLE → INDEXED → LINKED → ACCEPTED.
- Chỉ ACCEPTED khi các Gate tương ứng đã có Evidence và trạng thái được cập nhật trong hệ thống quản lý.

## 18. HƯỚNG DẪN NGƯỜI KẾ NHIỆM
- Khi tiếp nhận dự án: đọc tài liệu này trước; sau đó đọc MASTER_STATUS, PROJECT_SCHEDULE, DECISION_LOG, CHANGE_LOG, Data Dictionary, Dependency Matrix, Regression Matrix và Master Prompt phiên bản hiện hành.
- Không bắt đầu Mass Extraction chỉ vì có PDF. Kiểm tra Gate, Baseline, Prompt Version, Workbook/Schema và Data Contract.
- Khi gặp lỗi: không đoán, không che lỗi. Ghi nguồn, vị trí, mô tả, Evidence, trạng thái và chuyển theo Error/QA workflow.
- Khi gặp trường hợp chưa được quy định: giữ UNKNOWN/OPEN, tạo Change Request khi cần; không tự khóa một quy tắc mới.
- Khi không biết tài liệu nào là hiện hành: tra Version_History và Project Office logs; không chọn theo trí nhớ.

## 19. CHECKLIST TRƯỚC KHI CHẠY MỘT VOLUME
- Source Baseline đã xác nhận.
- Prompt Version hiện hành đã xác định.
- Workbook/Schema Baseline đã xác định.
- Data Contract/Traceability fields đã xác định.
- Retrieval/Page/Anchor capability đủ cho phạm vi cần làm hoặc đã ghi rõ giới hạn.
- Output paths/artifacts đã xác định.
- QA/Regression checklist đã sẵn sàng.
- Không có Critical Dependency đang BLOCKED mà vẫn bị bỏ qua.

## 20. CHECKLIST SAU KHI CHẠY
- Markdown tồn tại và có Traceability.
- Workbook/Index được tạo đúng Schema.
- Entities/Relationships/Cross-reference có Evidence.
- 05 vòng QA có log.
- Lỗi và UNKNOWN được giữ đúng trạng thái.
- Revision/Change history được lưu.
- Definition of Done được kiểm.
- Project Office nhận báo cáo Status → Done → Problem → Decision Needed → Next.

## 21. CÁC ĐIỂM CẦN ĐƯỢC XÁC NHẬN TRƯỚC FROZEN v1.0
- Xác nhận Workbook Schema/Baseline chính thức và xử lý mọi khác biệt giữa cấu trúc lịch sử và Workbook đang dùng.
- Xác nhận Data Dictionary và ID conventions đầy đủ.
- Đối chiếu Error Taxonomy/Regression Matrix với toàn bộ bảng lỗi LSVN_001 và TTVHVN_039.
- Xác nhận chính xác 05 vòng QA và trách nhiệm sửa/duyệt ở từng vòng.
- Xác nhận Data Contract với Website.
- Xác nhận vị trí chính thức của các file trong Documents/GitHub.
- Xác nhận nội dung Master Prompt thực thi tương ứng với Specification.
- Đây là các mục OPEN/TBD của Candidate; không được coi là đã khóa chỉ vì xuất hiện trong bản nháp lịch sử.

## 22. LỊCH SỬ PHIÊN BẢN
- v1.0-CANDIDATE — 24/08/2026: hợp nhất các nguyên tắc, quy trình, QA, Regression, Project Control và cơ chế mở rộng từ các tài liệu đã cung cấp; chưa Freeze.
- v1.0-FROZEN — chỉ phát hành sau khi hai bên duyệt và các mục OPEN/TBD quan trọng được giải quyết.
- v1.1+ — dùng cho các thay đổi có kiểm soát sau Freeze.

## PHỤ LỤC A — MẪU TASK
```text
OWNER:
TASK:
INPUT:
OUTPUT:
DEADLINE:
ACCEPTANCE:
DEPENDENCY:
NEXT ACTION IF OVERDUE:
STATUS:
```

## PHỤ LỤC B — MẪU DAILY STATUS
```text
STATUS:
DONE:
WAITING FOR:
OWNER:
DEADLINE:
PROBLEM / RISK:
DECISION NEEDED:
NEXT ACTION:
ESCALATION:
```

============================================================
23. ERROR → ACTION → ACCEPTANCE PROTOCOL
QUY TRÌNH XỬ LÝ LỖI TỪ PHÁT HIỆN ĐẾN NGHIỆM THU
============================================================

MỤC ĐÍCH

Chương này quy định cách xử lý một lỗi cụ thể từ khi phát hiện
cho đến khi có kết luận PASS / HOLD / BLOCKED.

Mục tiêu là trả lời rõ 07 câu hỏi:

1. Gặp lỗi này → mở đâu?
2. Kiểm tra gì?
3. Ghi ở đâu?
4. Ai xử lý?
5. Khi nào được PASS?
6. Khi nào phải HOLD / BLOCKED?
7. Khi nào lỗi phải chuyển thành Regression Test hoặc Change Request?


------------------------------------------------------------
23.1. QUY TẮC GỐC
------------------------------------------------------------

Khi phát hiện bất kỳ lỗi nào:

KHÔNG SỬA NGAY.

Phải đi theo chuỗi:

PHÁT HIỆN
→ ĐỊNH VỊ
→ PHÂN LOẠI
→ GHI NHẬN
→ XÁC ĐỊNH NGUYÊN NHÂN
→ XÁC ĐỊNH OWNER
→ XỬ LÝ
→ RE-CHECK
→ REGRESSION KHI CẦN
→ PASS / HOLD / BLOCKED

Không được bỏ qua bước GHI NHẬN chỉ vì lỗi có vẻ nhỏ.

Không được coi một lỗi là PASS chỉ vì đã sửa.

PASS phải có Evidence.


------------------------------------------------------------
23.2. LỖI NỘI DUNG / OCR
------------------------------------------------------------

Ví dụ:

PDF ghi đúng nhưng Markdown có:
- Sai chữ.
- Sai dấu.
- Sai số.
- Sai năm.
- Sai từ.
- Ký tự bị mất hoặc biến dạng.

MỞ:

SOURCE PDF
+
MARKDOWN ARTIFACT

KIỂM TRA:

- Trang PDF.
- Vị trí đoạn.
- Nội dung nguyên bản.
- Markdown tương ứng.
- Lỗi có phải OCR corruption không?
- Lỗi có phải Extraction Rule không?
- Lỗi có lặp lại theo Pattern không?

GHI:

DATA_QUALITY_LOG

Nếu lỗi có tính hệ thống:

ERROR_TAXONOMY
+
REGRESSION_TEST

OWNER:

DATA / EXTRACTION

PASS KHI:

- Markdown khớp nguồn.
- Evidence được lưu.
- Không còn sai lệch tại vị trí đã kiểm tra.

PHẢI HOLD / BLOCKED KHI:

- Không xác định được nội dung gốc.
- Nguồn không đủ bằng chứng.
- Cùng một Pattern xuất hiện trên nhiều Artifact.
- Có nguy cơ lỗi lan sang nhiều Volume.


------------------------------------------------------------
23.3. LỖI MẤT TRANG / PAGE MAPPING
------------------------------------------------------------

MỞ:

PDF
+
MARKDOWN
+
TRACEABILITY / PAGE MAPPING

KIỂM TRA:

PRINTED PAGE
↔
PDF PAGE
↔
MARKDOWN

KIỂM TRA THÊM:

- Có trang nguồn nhưng Markdown không có không?
- Page Number có bị lệch không?
- Printed Page có bị nhầm với PDF Page không?
- Có thể truy ngược từ Markdown về đúng trang nguồn không?

GHI:

DATA_QUALITY_LOG
+
TRACEABILITY FINDING

OWNER:

DATA / EXTRACTION

PASS KHI:

Có thể xác định đúng quan hệ:

Printed Page
↔
PDF Page
↔
Markdown Location

và có Evidence.

PHẢI HOLD / BLOCKED KHI:

- Không xác định được Page Mapping.
- Mapping sai trên diện rộng.
- Không thể bảo đảm Traceability.


------------------------------------------------------------
23.4. LỖI ANCHOR / RELATIVE LINK
------------------------------------------------------------

MỞ:

MARKDOWN
+
ANCHOR_ID
+
RELATIVE_LINK

KIỂM TRA:

1. Anchor có tồn tại không?
2. Anchor có duy nhất không?
3. Link có Destination không?
4. Destination có thực sự tồn tại không?
5. Link có hoạt động không?
6. Link có dẫn đúng Artifact không?
7. Có Reverse Traceability khi yêu cầu không?

GHI:

DATA_QUALITY_LOG
+
LINK_VALIDATION / REGRESSION

OWNER:

DATA / EXTRACTION

hoặc

TECHNICAL / DATA INTEGRATION

tùy nguyên nhân.

PASS KHI:

Link thực sự hoạt động và dẫn đúng Artifact.

Không được coi:

"Workbook có ghi Link"

là đủ điều kiện PASS.

PHẢI HOLD / BLOCKED KHI:

Lỗi Link / Anchor xuất hiện theo Pattern ở nhiều Artifact
hoặc làm mất Traceability.


------------------------------------------------------------
23.5. LỖI ENTITY
------------------------------------------------------------

Ví dụ:

- Entity bị nhận dạng sai.
- Entity bị thiếu.
- Entity bị trùng.
- Entity bị gán nhầm loại.
- Entity không có Source Evidence.

MỞ:

MARKDOWN
→
ENTITY SHEET
→
DATA DICTIONARY

KIỂM TRA:

- Entity Name.
- Entity ID.
- Alias.
- Entity Type.
- Person / Work / Place / Event / Dynasty...
- Có trùng Entity không?
- Có nhầm Entity khác không?
- Source Evidence ở đâu?

GHI:

DATA_QUALITY_LOG

Nếu lỗi lặp lại:

REGRESSION_TEST

OWNER:

DATA / EXTRACTION

PASS KHI:

- Entity đúng.
- Không trùng.
- ID hợp lệ.
- Có Source Evidence.
- Entity Type đúng.

PHẢI HOLD / BLOCKED KHI:

- Không xác định được Entity từ nguồn.
- Cần thay đổi Entity Schema.
- Cần thay đổi quy tắc nhận dạng chung.


------------------------------------------------------------
23.6. LỖI WORKBOOK / INDEX
------------------------------------------------------------

Ví dụ:

Markdown có thông tin nhưng Workbook / Index không có.

MỞ:

MARKDOWN
→
WORKBOOK / INDEX
→
DATA DICTIONARY
→
DEPENDENCY MATRIX

KIỂM TRA:

- Dữ liệu có tồn tại trong Markdown không?
- Có Field tương ứng trong Workbook không?
- Đã ghi đúng Sheet chưa?
- Đúng ID chưa?
- Sheet là Source Sheet hay Derived Sheet?
- Có vi phạm Dependency không?
- Dữ liệu có truy xuất được về nguồn không?

GHI:

DATA_QUALITY_LOG

Nếu nguyên nhân là Schema:

CHANGE_REQUEST

OWNER:

DATA / EXTRACTION

PASS KHI:

- Đúng Sheet.
- Đúng Field.
- Đúng ID.
- Có Source Evidence.
- Không phá vỡ Dependency.

PHẢI HOLD / BLOCKED KHI:

Schema hiện tại không thể chứa dữ liệu cần thiết.

KHÔNG tự ý thêm Column / Sheet vào Baseline đã khóa.

Phải tạo:

SCHEMA CHANGE REQUEST


------------------------------------------------------------
23.7. LỖI SEARCH / HOVER
------------------------------------------------------------

MỞ:

WORKBOOK
→
SEARCH / HOVER
→
MARKDOWN
→
DATA CONTRACT

KIỂM TRA:

- Entity có tồn tại không?
- ID có đúng không?
- Search Index có Record không?
- Hover có dữ liệu không?
- Link về nguồn có hoạt động không?
- Website có đọc đúng Data Contract không?

GHI:

DATA_QUALITY_LOG

PHÂN LOẠI:

Nếu lỗi nằm ở Data:
→ DATA ISSUE

Nếu lỗi nằm ở Website:
→ WEBSITE ISSUE

Không sửa Data chỉ để che một lỗi UI.

PASS KHI:

Search / Hover trả đúng thông tin,
đúng Entity,
đúng ID,
và có Traceability theo yêu cầu.

PHẢI HOLD / BLOCKED KHI:

Data Contract không đủ để Website thực hiện chức năng.


------------------------------------------------------------
23.8. LỖI CROSS-REFERENCE / RELATIONSHIP
------------------------------------------------------------

MỞ:

MARKDOWN
→
CROSS_REFERENCE
→
RELATIONSHIP
→
ENTITY

KIỂM TRA:

- Source Entity.
- Target Entity.
- Relationship Type.
- Evidence.
- Link.
- Có vòng lặp sai không?
- Có Entity mồ côi không?
- Quan hệ có thực sự được nguồn hỗ trợ không?

GHI:

DATA_QUALITY_LOG
+
RELATIONSHIP_REGRESSION

OWNER:

DATA / EXTRACTION

PASS KHI:

Quan hệ có Evidence và truy xuất được theo yêu cầu.

PHẢI HOLD / BLOCKED KHI:

Không đủ nguồn để xác định quan hệ.

KHÔNG SUY LUẬN.

Nếu nguồn không đủ:

UNKNOWN


------------------------------------------------------------
23.9. LỖI "KHÔNG BIẾT"
------------------------------------------------------------

Nếu người thực hiện không biết phải xử lý thế nào:

KHÔNG ĐƯỢC TỰ QUYẾT ĐỊNH.

Ghi:

UNKNOWN
hoặc
OPEN
hoặc
NEEDS REVIEW

Sau đó xác định nguyên nhân:

Nếu Specification chưa quy định:
→ CANDIDATE VALIDATION FINDING

Nếu Prompt chưa quy định:
→ PROMPT CHANGE REQUEST

Nếu Schema chưa đủ:
→ SCHEMA CHANGE REQUEST

Nếu nguồn không đủ:
→ SOURCE UNKNOWN

Nếu Website không hỗ trợ:
→ DATA CONTRACT / WEBSITE ISSUE

Nếu ảnh hưởng Critical Path:
→ PROJECT OFFICE REVIEW


------------------------------------------------------------
23.10. KHI NÀO KHÔNG CẦN DỪNG
------------------------------------------------------------

Không cần dừng toàn bộ công việc nếu:

- Lỗi đơn lẻ.
- Nguyên nhân rõ.
- Nguồn xác định được.
- Cách xử lý đã có trong Specification.
- Không ảnh hưởng hệ thống.
- Không lan sang Artifact khác.
- Có thể xử lý và Re-check độc lập.

QUY TRÌNH:

GHI NHẬN
→
XỬ LÝ
→
RE-CHECK
→
PASS
→
TIẾP TỤC


------------------------------------------------------------
23.11. KHI NÀO PHẢI HOLD / BLOCKED
------------------------------------------------------------

PHẢI HOLD / BLOCKED khi xảy ra một trong các trường hợp:

1. LỖI HỆ THỐNG

Cùng một Pattern xuất hiện ở nhiều nơi.

2. LỖI SOURCE / RETRIEVAL

Không xác định được nguồn hoặc Page Mapping.

3. LỖI SCHEMA

Workbook / Data Model không chứa được dữ liệu cần thiết.

4. LỖI PROMPT

Prompt tạo ra cùng một lỗi lặp lại.

5. LỖI TRACEABILITY

Không thể truy ngược dữ liệu về nguồn.

6. LỖI CRITICAL

Có nguy cơ làm sai hàng loạt Artifact.

7. CHƯA CÓ CÁCH XỬ LÝ

Vấn đề ảnh hưởng đến Critical Path.

Khi đó:

HOLD / BLOCKED

Không tự tìm cách "lách".


------------------------------------------------------------
23.12. CÁC TRƯỜNG HỢP AI KHÔNG ĐƯỢC TỰ QUYẾT ĐỊNH
------------------------------------------------------------

AI không tự quyết định khi:

- Nguồn không đủ.
- Hai nguồn mâu thuẫn.
- Schema không đủ.
- Prompt chưa quy định.
- Lỗi có khả năng hệ thống.
- Thay đổi Data Contract.
- Thay đổi Baseline.
- Thay đổi Master Prompt.
- Thay đổi cấu trúc Workbook.

Các trường hợp này phải chuyển:

OPEN
→
PROJECT OFFICE REVIEW


------------------------------------------------------------
23.13. PHÂN VAI XỬ LÝ
------------------------------------------------------------

QA:

- Phát hiện.
- Định vị.
- Phân loại.
- Ghi nhận.
- Đánh giá PASS / FAIL / HOLD.

DATA / EXTRACTION:

- Điều tra nguyên nhân.
- Sửa Artifact theo quy trình được phép.
- Tạo lại Output khi cần.
- Cung cấp Evidence.

PROMPT:

- Xử lý khi nguyên nhân nằm ở Prompt / Rule.
- Đề xuất Prompt Change Request.
- Thực hiện Regression sau thay đổi.

SCHEMA / DATA:

- Xử lý khi nguyên nhân nằm ở Workbook / Data Model.
- Đề xuất Schema Change Request khi cần.

WEBSITE:

- Xử lý khi lỗi nằm ở Presentation / API / UI.
- Không tự thay đổi Source Data để chữa lỗi UI.

PROJECT OFFICE:

Quyết định khi có:

- Thay đổi Baseline.
- Thay đổi Prompt.
- Thay đổi Schema.
- Thay đổi Data Contract.
- Lỗi Critical.
- HOLD / GO.
- Golden Acceptance.


------------------------------------------------------------
23.14. THẺ XỬ LÝ LỖI NHANH
------------------------------------------------------------

GẶP LỖI
↓
MỞ NGUỒN + ARTIFACT
↓
XÁC ĐỊNH VỊ TRÍ
↓
PHÂN LOẠI
↓
GHI DATA_QUALITY_LOG
↓
XÁC ĐỊNH OWNER
↓
LỖI ĐƠN LẺ?
    |
    +-- CÓ
    |     ↓
    |   XỬ LÝ
    |     ↓
    |   RE-CHECK
    |     ↓
    |   PASS
    |     ↓
    |   TIẾP TỤC
    |
    +-- KHÔNG
          ↓
       CÓ TÍNH HỆ THỐNG?
          |
          +-- CÓ → HOLD / BLOCKED
          |
          +-- KHÔNG
                ↓
         XÁC ĐỊNH NGUYÊN NHÂN
                ↓
       SOURCE / PROMPT / SCHEMA /
       DATA / WEBSITE / QA
                ↓
          CHANGE REQUEST
             KHI CẦN
                ↓
           REGRESSION
             KHI CẦN
                ↓
          PASS / FAIL / HOLD


------------------------------------------------------------
23.15. BẢY NGUYÊN TẮC NHỚ NHANH
------------------------------------------------------------

1. KHÔNG BIẾT XỬ LÝ
   ≠
   ĐƯỢC PHÉP TỰ ĐOÁN.

2. KHÔNG CHẮC CHẮN
   ≠
   PASS.

3. LỖI HỆ THỐNG
   ≠
   SỬA TỪNG DÒNG RỒI TIẾP TỤC.

4. THAY ĐỔI QUY TẮC
   ≠
   SỬA PROMPT TRỰC TIẾP.

5. PASS
   =
   CÓ EVIDENCE.

6. BLOCKED
   =
   CÓ OWNER + NEXT ACTION + ESCALATION.

7. LỖI MỚI
   =
   CƠ HỘI BỔ SUNG HỆ THỐNG,
   KHÔNG PHẢI LÝ DO ĐỂ DỰ ÁN BẾ TẮC.


------------------------------------------------------------
23.16. MẪU ERROR RECORD
------------------------------------------------------------

ERROR_ID:

DATE:

ARTIFACT:

VOLUME:

SOURCE_PAGE:

ARTIFACT_LOCATION:

ERROR_TYPE:

DESCRIPTION:

EVIDENCE:

EXPECTED:

ACTUAL:

ROOT_CAUSE:

OWNER:

ACTION:

REGRESSION_REQUIRED:
YES / NO

CHANGE_REQUEST_REQUIRED:
YES / NO

STATUS:

OPEN
IN_PROGRESS
PASS
FAILED
HOLD
BLOCKED

NEXT_ACTION:

ESCALATION:

DATE_CLOSED:

CLOSING_EVIDENCE:


------------------------------------------------------------
23.17. ĐIỀU KIỆN ĐÓNG LỖI
------------------------------------------------------------

Một lỗi chỉ được đóng khi:

1. Nguyên nhân đã được xác định hoặc được chấp nhận là UNKNOWN
   theo quy trình.

2. Action đã được thực hiện.

3. Artifact đã được Re-check.

4. Evidence được lưu.

5. Không còn sai lệch tại vị trí kiểm tra.

6. Nếu lỗi có tính hệ thống:
   Regression Test đã được thực hiện.

7. Nếu có thay đổi Prompt / Schema / Data Contract:
   Change Request đã được xử lý và phiên bản liên quan
   đã được cập nhật.

8. Project Office / Owner có thẩm quyền đã xác nhận trạng thái
   khi lỗi thuộc Critical Path hoặc Acceptance Gate.

KẾT QUẢ CUỐI:

PASS
hoặc
HOLD
hoặc
BLOCKED

Không đóng lỗi bằng trạng thái mơ hồ.


------------------------------------------------------------
23.18. NGUYÊN TẮC DÙNG CHƯƠNG NÀY TRONG VALIDATION
------------------------------------------------------------

Chương này phải được kiểm chứng bằng các lỗi thực tế của:

LSVN_001
+
TTVHVN_039

Mỗi lỗi thực tế phải có thể trả lời được:

GẶP LỖI NÀY
→
MỞ ĐÂU
→
KIỂM TRA GÌ
→
GHI Ở ĐÂU
→
AI XỬ LÝ
→
PASS KHI NÀO
→
DỪNG KHI NÀO

Nếu một lỗi thực tế không thể đi hết chuỗi trên,
đó là:

CANDIDATE VALIDATION FINDING

và phải được dùng để tiếp tục hoàn thiện Specification.

============================================================
HẾT CHƯƠNG 23
============================================================
