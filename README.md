# Convension
CONVENSION 2024: 
(C#) :
Database :
Namespace phải đặt theo đúng vị trí.  Vd : namespace DataAccess.Databases.Cmms.
Thuộc tính của class viết bằng Upper CamelCase , viết đúng chính tả.
Dto:
Các type phải về cùng 1 cấp , ngoại trừ một số kiểu dử liệu : mảng,..

Repo:
Tên các file, interface, implement đặt theo tính năng chính , lược bỏ tiền tố mudule . Ví dụ :MasterVendorRepo  => VendorRepo.
Viết chung tất cả trong 1  folder
Service: 
Tên các file, interface, implement đặt theo tính năng chính , lược bỏ tiền tố mudule . Ví dụ :MasterVendorRepo  => VendorRepo.

Thêm Async làm hậu tố cho các tác vụ bất đồng bộ ví dụ : GetAll -> GetAllAsync.



Lower Camel-case : 
Parameter
Type Req: Chia ra type createReq , updateReq đối với từng trường hợp.


Controller: 
Bắt buộc Authorize trong Controller, Belong toTenantFilter.
Bắt buộc Response Type 
Unit Test 


FrontEnd : 
Bỏ tiền tố Module.
Đặt tên component theo feature ( Tên file , tên component, tên selector).
Tên biến : kiểu mảng ( thêm s phía sau ) , bool là isSomething, scope private _.
Router : kebab-case (vd: vendor).
Unsubcribe all khi destroy component.
Tên function - động từ.
Navigate route có mà phiếu - encode Url component.
Inject 



