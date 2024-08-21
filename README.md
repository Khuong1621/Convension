
# Coding Convention (C#)


- Thay đổi cách đặt tên: tên class database sẽ mang tiền tố là tên Module ,các tên ở các lớp khác sẽ lược bỏ phần tiền tố đi.(kể cả tên file)
- Gộp tất cả các module về cùng một folder chứa , có tên Master.

## Database
- Namespace phải đặt đúng vị trí , project , folder.
- Thuộc tính phải đặt tên đúng chính tả và đặt bằng tiếng Anh.
```csharp
namespace DataAccess.Databases.Master;

public class MasterVendor
{
    public string Id { get; set; }
    public string Name { get; set; }
    public string Address { get; set; }
}
```
- Type phải phân loại ứng với từng action  : CreateReq , UpdateReq.
- Type phải được đưa về đồng cấp , ngoại trừ kiểu mảng . Và cách dặt tên tương ứng ví dụ sau:

```csharp
public class VendorRes
{
    public string Id { get; set; }
    public string Name { get; set; }
    public string Address { get; set; }

    public string CreatedUserName { get; set; }

}
```
## Service và Repo
- Với tác vụ bất đồng bộ bắt buộc dùng async , await. Tên phương thức phải mang hậu tố là Async .
- Các đối số của hàm , phải trình bày dưới dạng Lower Camel -case

```csharp
public interface ICmmsAssetActivityService : IDomainService
{
    Task<CmmsAssetActivityStatusReportRes> GetAllAsync(string assetId, DateTime start, DateTime end);
    Task<CmmsAssetActivityStatusRes> CreateAsync(string assetId, CmmsAssetActivityStatusReq req);
    Task<bool> DeleteAsync(string assetId, string assetActivityStatusId);
}
```
## Controller

- Mỗi controller là một tính năng riêng biệt.
- Bắt buộc dùng authorize , ngoại trừ một số trường hợp đặc biệt phải xin phép trước.
- BelongToTenantfilter là mặc định ở mỗi Controller, không có ngoại lệ.
- Tương tự ,với Respontype của api phải có thì mới hoàn thành 1 api.(+ UnitTest).

```csharp
[Authorize]
[Route("api/cmms/assets")]
[ServiceFilter(typeof(BelongToTenantFilter))]
public class CmmsAssetController : ApiController
```





# Coding Convention (FE)

- Bỏ tiền tố là tên Module , viết ngắn gọn tên tính năng.
- Đặt tên component theo feature , tương tự với tên file , tên selector theo định dạng kebab-case.
- Biến : 
    +   Đặt tên bắt buộc là kiểu lower Camel-case: assetType, unsubcribeAll$ .
    +   Kiểu mảng(gắn thêm s phía sau): locatons, assets, activities.
    +   Kiểu bool(mang tiền tố là is): isUse, isActive.
    +   Một số trường hợp phải trình bày :Kiểu enum(ghi rõ những giá trị cụ thể).
    +   Phạm vi (scope) là private , chỉ sử dụng ở component thì thêm '_' vào trước tên: _transloco , _leaflet.

- Hàm (Function):
    +   Đặt tên bắt buộc là kiểu lower Camel-case 
    +   Đặt tên có ý nghĩa, phải là động từ với tất cả các hàm.
    +   Mỗi hàm chỉ thực hiện 1 nhiệm vụ theo tên hàm , nếu phức tạp thì chia nhỏ logic.
    +   Input , Output của hàm , đều phải chỉ định type.
- Route: 
    +   Đặt tên bắt buộc là kiểu lower Camel-case.
    +   Navigate tới route có Mã thì phải có encode. 
- Quy tắc chung: 
    +   Inject Service phải có unsubcribeAll ở giai đoạn destroy của component.
    +   Mỗi tính năng là một module, routing riêng, đặc biệt hơn làm 1 component standalone.
    +   Biến loading là cần thiết đối với tác vụ cần chờ phản hồi từ server , cập nhật biến loading bằng .fnalize(Rxjs).
    +   Mat component sẽ là thành phần chính. Màu primary thì phải ghi primary , phân biệt với màu thường.
    +   Nztable phân trang , index phải đi cùng với PageSize và PageNumber.
    +   Hạn chế Import ShareModule , sẽ phải nạp module không cần thiết cho component.
- File việt hóa : 
    +   Đặt tên bắt buộc là kiểu lower Camel-case.
    +   Bổ sung vào từ điển vào vị trí cuối cùng ( tránh conflict dẫn đến mất từ).
    +   Bổ sung vào từ điển vào vị trí cuối cùng ( tránh conflict dẫn đến mất từ).
    +   Sử dụng lại từ đã có.
    +   Không phân level , ghi trực tiếp một dòng:
        ```csharp
        {
            "cmms":{
                "meter":"Đồng hồ đo",
            }
        }

        sửa lại
        {
            "cmms.meter":"Đồng hồ đo"
        }
        ```

        .   Phân biệt được từ khóa nào dùng chung , hoặc đặc thù cho module đó("error","placeholder","cmms","system","message"): 
         ```csharp
        {
            "cmms.meter":"Đồng hồ đo",
            "createDate":"Ngày tạo", 
            "master.vendor":"Nhà cung cấp"
        }
        ```
- Mã tự sinh : không sinh ở Front End .

- Services và Type:
    + Đặt tên định dạng Upper CamelCase, tên file : kebab-case.
    + Đặt đúng tên như Controller khai báo, kể cả type.

- Popup :
    + Đặt tên định dạng Upper CamelCase, tên thì phải có hậu tố popup để phân biệt với component.




