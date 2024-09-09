# Cách đặt tên commmit
Link tham khảo(https://viblo.asia/p/lam-the-nao-de-viet-conventional-commits-cho-de-su-dung-07LKXbb2lV4)
Các thành phần type, description là bắt buộc cần có trong commit message, optional là tùy chọn có hoặc không có cũng được
- type: từ khóa để phân loại commit là feature, fix bug, refactor.
- scope: cũng được dùng để phân loại commit, vùng ảnh hưởng của commit, trả lời câu hỏi: commit này refactor|fix cái gì? được đặt trong cặp ngoặc đơn ngay sau type. VD: feat(authentication):, fix(parser):
-description: là mô tả ngắn về những gì sẽ bị sửa đổi trong commit đấy
- body: là mô tả dài và chi tiết hơn, cần thiết khi description chưa thể nói rõ hết được, có thể thêm phần ghi chú bằng các keyword
- footer: một số thông tin mở rộng như số ID của pull request, issue.. được quy định theo conventional
```
<type>[optional scope]: [< jira - task - id >] < description >

[optional body]

[optional footer]
```
Example
```
feat: [TSW-223] add validate of A feature

fix: [TSW-223] fix die dashboard page

feat(feature_a): [TSW-223] add validate of A1 feature

```
# Coding Convention (C#)

## UpperCamelCase
- Sử dụng cho đặt tên file.
- Sử dụng cho public methods và properties.
```csharp
public class DataService
{
    public string Id { get; set; }
    public string Name { get; set; }
    public string Address { get; set; }
}
```


-Sử dụng khi đặt tên `class`, `record`, hoặc `struct`.

```csharp
public class DataService
{
}
```

```csharp
public record PhysicalAddress(
    string street,
    string city,
    string stateOrProvince,
    string zipCode);
```

```csharp
public struct ValueCoordinate
{
}
```

Sử dụng khi đặt tên interface, thêm prefix `I` trước name.

```csharp
public interface IWorkerQueue
{
}
```

Sử dụng khi đặt tên cho `public` types, fields, properties, events, methods, và local functions.

```csharp
public class ExampleEvents
{
    // A public field, these should be used sparingly
    public bool IsValid;

    // An init-only property
    public IWorkerQueue WorkerQueue { get; init; }

    // An event
    public event Action EventProcessing;

    // Method
    public void StartEventProcessing()
    {
        // Local function
        static int CountQueueItems() => WorkerQueue.Count;
        // ...
    }
}
```
### UpperCamelCase
Sử dụng cho các scope variable.
```csharp
public async Task<int> Update(SystemHelpModel model)
{
    var name = model.Db.Name;
    var address = model.Db.Address;

    return 1;
}
```
### Camel case

Sử dụng khi `private` hoặc `internal` fields, và thêm prefix `_`.

```csharp
public class DataService
{
    private IWorkerQueue _workerQueue;
}
```

Sử dụng với `static` fields có AC là `private` hoặc `internal`, sử dụng prefix `s_` và cho thread static `t_`.

```csharp
public class DataService
{
    private static IWorkerQueue s_workerQueue;

    [ThreadStatic]
    private static TimeSpan t_timeSpan;
}
```

Sử dụng với `arguments` của `method`, sử dụng `lowerCamelCase`.

```csharp
public T SomeMethod<T>(int someNumber, bool isValid)
{
}
```

### Sử dụng Named Arguments trong việc gọi method:

When calling a method, arguments are passed with the parameter name followed by a colon and a value.

```csharp
// Method
public void DoSomething(string foo, int bar)
{
    ...
}

// Avoid
DoSomething("someString", 1);
// Correct
DoSomething(foo: "someString", bar: 1);
```

## Quy tắc comment

-Những hàm phức tạp cần mô tả rõ chức năng.
- Sử dụng `///` để mô tả chức năng của field. (VS Code Extension).
- Đối với `method` cần mô tả thêm ý nghĩa có giá trị trả về (nếu có)

```csharp
/// <summary>
/// id unit table
/// </summary>
/// <value></value>
[MaxLength(128)]
public string IdUnit { get; set; }
```

```csharp
/// <summary>
/// Func update record help
/// </summary>
/// <param name="model">SystemHelpModel</param>
/// <returns>1 if success</returns>
public async Task<int> Update(SystemHelpModel model)
{
    var db = await _context.SystemHelps.Where(d => d.Id ==  model.Db.Id).FirstOrDefaultAsync();
    db.Name = model.Db.Name;
    db.UpdateBy = model.Db.UpdateBy;
    db.UpdateDate = model.db.UpdateDate;
    _context.SaveChanges();
    return 1;
}
```

## Quy tắc liên quan EF

- Nên sử dụng `async` `Task` nếu có thể.
- Phần xử lý logic liên quan đến database bắt buộc viết tại Repository Layer.
- Sử dụng `[MaxLength(128)]` đối với column ID.
- Sử dụng `[MaxLength(200)]` đối với column Name.
- Sử dụng `[MaxLength(500)]` đối với column Note.
- Bắt buộc thêm `nullable` đối với column (trừ primary key).

```csharp
public int? Id {get; set;}
public string Name {get; set;}
public string Address {get; set;}
```

- Bắt buộc phải khai báo `type` của `field` khi tạo DB mới

```csharp
[MaxLength(128)]
public string IdUnit { get; set; }
```
## Quy tắc viết Unit Test
- Mỗi `Action` trong `Controller` bắt buộc phải có ít nhất 1 `Unit test`.
- Viết test case đúng chức năng của một `Action` trong `Controller`.
- Không sử dụng `decouple` `Action` trong `Controller` trong một Unit test.
```csharp
public class SystemItemTestController : InitHttpContextController<SystemItemController> 
{

    public SystemItemTestController() : base(new DatabaseFixture())
    {
        controller = new SystemItemController(userService: userService, context: fixture.context, serviceFactory: serviceFactory);
    }
    [Fact]
    public async Task GetItemByBarcodeAsync_ShouldReturn200Status()
    {
        // Arrange
        var result = (JsonResult)await controller.getItemByBarcode("", "");

        //Assert
        result.StatusCode.Should().NotBe(500);
    }
    [Fact]
    public async Task DeleteAsync_ShouldDeletedAndReturn200Status()
    {
        // Arrange
        var newId = Guid.NewGuid().ToString();
        fixture.context.SystemItems.Add(new SystemItemDb
        {
            Id = newId
        });

        var identity = new GenericIdentity("TestUser", "UnitTest");
        var contextUser = new ClaimsPrincipal(identity); 
        controller.ControllerContext = new ControllerContext
        {
            HttpContext = new DefaultHttpContext { User = contextUser }
        };

        await fixture.context.SaveChangesAsync();
        // Act
        var result = controller.Delete(JObject.FromObject(new { Id = newId }));

        // Assert
        var record = await fixture.context.SystemItems.Where(d => d.Id == newId).SingleOrDefaultAsync();
        Assert.Equal(expected :2,actual: record?StatusDel);
        result.StatusCode.Should().Be(200);
        result.StatusCode.Should().NotBe(500);
    }
}
```
## Quy tắc khác

- Khi `method` bị khai báo quá 3 lần thì nên viết lại tại `Helpers`
- Đặt tên file theo `UpperCamelCase` và có suffix (`Repo`, `Model`, `Part`, `Controller`, `Db`)
- Đặt tên file theo `UpperCamelCase` và có prefix là tên Module

```csharp
SysItemRepo.cs
SysItemModel.cs
SysItemPart.cs
InventoryItemModel.cs
```
```csharp
/// <summary>
/// Lấy dữ liệu.
/// </summary>
[HttpGet]


/// <summary>
/// Tạo mới dữ liệu.
/// </summary>
[HttpPost]


/// <summary>
/// Cập nhật 1 field.
/// </summary>
[HttpPatch]


/// <summary>
/// Cập nhật nhiều field.
/// </summary>
[HttpPut]


/// <summary>
/// Xóa record.
/// </summary>
[HttpDelete]


```
- Controller chỉ chức business logic không tương tác với Database
- Controller bắt buộc phải có Http Method và cần mô tả chi tiết prams, chức năng của controller

```csharp
/// <summary>
/// Get list of helps.
/// </summary>
/// <param name="item"></param>
/// <returns>Returns the array of helps</returns>
/// <remarks>
/// Sample request:
///
///     POST /Todo
///     {
///        "Id": 1,
///        "Name": "Item #1",
///        "IsComplete": true
///     }
///
/// </remarks>
/// <response code="200">Returns the array of helps</response>
[HttpPost]
[ProducesResponseType(typeof(IEnumerable<CmSelectStringModel>),StatusCodes.Status200Created)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
public IActionResult GetListUse()
{
    var result = repo._context.SystemHelps
        .Where(d=>d.StatusDel==1)
            .Select(d => new CmSelectStringModel
            {
                Id = d.Id,
                Name = d.Name
            }).ToList();
    return Json(result);
}

```

## Cấu trúc source .Net + Angular
- Repository: `Chứa những method cơ bản (CRUD) liên quan đến database`, có thể bổ sung thêm method nếu cần thiết. `Không được viết business logic`.
- Service: `Chứa những method liên quan đến business logic`, có thể bổ sung thêm method nếu cần thiết. `Giao tiếp trực tiếp với tầng Repository`.
- Controller: `Sử dụng các method Service`. `Không giao tiếp trực tiếp với Repository`.`Không chứa business logic`.
```
    SmartFactory
    ├── Api
    │   └── Applications
    │   │  └── ClientApp                                        # Angular
    ├── CrossCutting
    │   ├── CrossCutting.IoC
    │   │   └── NativeInjector.cs                               # Khai báo DI
    │   ├── CrossCutting.Bus                               
    ├── Infrastructure                                      
    │   ├── Core
    │   ├── DataAccess
    │   │   │   ├── Contexts 
    │   │   │   │   │   ├── SystemDbContext.cs                  # Thêm các bảng của module System
    │   │   │   │   │   ├── ApplicationDbContext.cs            
    │   │   │   │   │   └── ...   
    │   │   │   ├── Databases 
    │   │   │   │   ├── System 
    │   │   │   │   │   ├── SystemCommonCodeDb.cs               # Danh sách bảng của database module System
    │   │   │   │   │   ├── SystemPageItemDb.cs
    │   │   │   │   │   └── ... 
    │   │   │   └── ...   
    ├── Modules  
    │   ├── Systems                                 
    │   │   │   ├── Appplication                        
    │   │   │   │   ├── Controllers 
    │   │   │   │   │   ├── SystemAccountController.cs          # Controller
    │   │   │   │   │   └── ...
    │   │   │   ├── Domain
    │   │   │   │   ├── Services 
    │   │   │   │   │   ├── SystemAccountService.cs             # Mỗi Controller sẽ khai báo ít nhất 1 service
    │   │   │   │   │   └── ...
    │   │   │   ├── Infrastructure
    │   │   │   │   ├── Interfaces
    │   │   │   │   │   ├── ISystemUserRepo.cs                  # Khai báo Interface cho Repository, Service
    │   │   │   │   │   └── ISystemAccountService.cs
    │   │   │   │   ├── Models
    │   │   │   │   │   ├── SystemUserModel.cs                  # Khai báo Model cho module System.
    │   │   │   │   │   └── SystemGetUserModel.cs
    │   │   │   │   ├── Repositories
    │   │   │   │   │   ├── SystemUserRepo.cs                   # Mỗi table (DB) đều có 1 repo riêng.
    │   └── ...
```
# Coding Convention (Typescript)

| Style          | Category                                                           |
| :------------- | :----------------------------------------------------------------- |
| UpperCamelCase | class / interface / type / enum / decorator / type parameters      |
| lowerCamelCase | variable / parameter / function / method / property / module alias |
| CONSTANT_CASE  | global constant values, including enum values                      |
| #ident         | private identifiers are never used.                                |

```typescript
const UNIT_SUFFIXES = {
  milliseconds: "ms",
  seconds: "s",
};
```

- Khi so sánh bằng bắt buộc sử dụng `===`

```typescript
a === b; // Good
a == b; // Bad
```

- Bắt buộc sử dụng `arrow function` của ES6

```typescript
bar(() => { this.doSomething(); }) // Good

bar(function() { ... }) // Bad
```

- Sử dụng single quote

```typescript
const foo = "bar"; // Good
const foo = "bar"; // Bad
```

- Chỉ sử dụng {} khi đúng trường hợp

```typescript
myPromise.then((v) => console.log(v)); // Bad

myPromise.then((v) => {
  console.log(v);
}); // Good
```

- Bắt buộc phải khai báo và sử dụng type (trừ những trường hợp đặc biệt sử dụng `any`)
- Đặt tên file mới theo đúng cấu trúc (`lowerCamelCase`)
- Tên file, folder sẽ đặt theo chuẩn (`kebab-case`)
- Toàn bộ API phải đặt trong file `index.service.ts`, `Không` được viết trong file component
  >
      .
      ├── ...
      ├── system-item                  # Tên trang có prefix tên module system
      │   ├── export.ts               # Export chung của tất cả component trong page
      │   ├── index.component.ts      # Component chính
      │   ├── index.component.html    # Giao diện của component chính
      │   ├── popup-add.component.ts   # Popup component liên quan xem, sửa, xoá
      │   ├── popup-add.component.html # Giao diện popup component liên quan xem, sửa, xoá
      │   ├── types.ts                # Khai báo type cho toàn bộ page
      │   ...
      │   ├── index.service.ts        # Service để cho các component khác sử dụng
      │   └── index.resolver.ts       # Resolver để lấy dữ liệu trước khi render component
      ├── system.module.ts            # Module system
      ├── system.routing.ts           # Routing Module system
      └── system.helper.ts            # Helper (các hàm sử dụng lại)
- Khi khai báo trong component trong module, sử dụng `import` từ `./export`, không import trực tiếp

```typescript
import { inventory_report_position_map_indexComponent } from "./inventory_report_position_map/export"; // Good
import { inventory_report_position_map_indexComponent } from "./inventory_report_position_map/index.component"; // Bad
```

- Bắt buộc sử dụng `Tailwind CSS`, hạn chế sử dụng css thuần.

## Quy tắc comment

- Mô tả nội dung của `function`, `variable` theo như mẫu

```typescript
/**
 * Get navigation component from the registry
 *
 * @param name
 */
getComponent(name: string) : void
{
    return this._componentRegistry.get(name);
}
```

# Coding Convention (Dart - Flutter)

- Classes, enum types, typedefs, và type parameters thì sử dụng `UpperCamelCase`.

```dart
class SliderMenu { ... }

class HttpRequest { ... }

typedef Predicate<T> = bool Function(T value);
```

- Tên libraries, packages, directories, và source files sử dụng `lowercase_with_underscores`.

```dart
library peg_parser.source_scanner;

import 'file_system.dart';
import 'slider_menu.dart';
```

- Trường hợp còn lại sử dụng `lowerCamelCase`.

```dart
var count = 3;

HttpRequest httpRequest;

void align(bool clearItems) {
  // ...
}
```

- Sử dụng `single quote`

```dart
var foo = 'bar'; // Good
var foo = "bar"; // Bad
```

- Sắp xếp `import` đúng thứ tự

```dart
import 'dart:async';
import 'dart:html';

import 'package:bar/bar.dart';
import 'package:foo/foo.dart';
```

- Comment mô tả chức năng của các `Functions`, `Properties`.

````dart
/// URL avatar.
///
/// ```dart
/// var example = CodeBlockExample();
/// print(example.isItGreat); // "Yes."
/// ```
/// ABC XYZ.
String? avatarPath;
````

## Cấu trúc source

>

      lib
      ├── app
      ├── common
      │   ├── config
      │   ├── constant                                  # Biến dùng chung
      │   ├── model                                     # Model của toàn bộ app
      │   ├── preference                                # Function shared_preference
      │   ├── route                                     # Route cho toàn bộ app
      │   ├── util                                      # Các Function sử dụng lại
      │   └── widget
      ├── feature                                       # Danh sách các tính năng của app
      │   ├── login
      │   │   ├── bloc                                  # Bloc của tính năng (nhiều bloc khác nhau)
      │   │   │   ├── export.dart                       # Export chung của Bloc
      │   │   │   ├── login_bloc.dart                   # Bloc xử lý của login feature
      │   │   │   ├── login_event.dart                  # Event Bloc của login feature
      │   │   │   └── login_state.dart                  # State Bloc của login feature
      │   │   ├── repository
      │   │   │   └── login_repository.dart             # Repository (sử dụng để call API Login)
      │   │   └── screen
      │   │   │   └── login.dart                        # Screen chính login tổng hợp từ các widget
      │   │   └── widget
      │   │       └── button_login.dart                 # Widget của screen login
      │   └── ...
      └── translations                                  # Cấu hình đa ngôn ngữ



# Update Convention - 18/8/2024 (C#)


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



# Unit Test Require
## Các API CRUD cơ bản :
1.	Create (POST)
2.	Read (GET)
3.	Update (PUT/PATCH)
4.	Delete (DELETE)
## Số lượng unit test cho mỗi API
Create (POST)

- File việt hóa 

- Kiểm tra thành công: Thêm mới một bản ghi hợp lệ

- Kiểm tra thất bại: Gửi dữ liệu không hợp lệ 
- Kiểm tra với dữ liệu trùng lặp 



Số lượng ước tính: 2-3 tests



```csharp



```
Read (GET)
- Kiểm tra thành công: Lấy dữ liệu khi có bản ghi


```csharp

        [TestMethod()]
        public async Task GetAll_Success_Test()
        {
            // Act
            var actual = await _controller.GetAll(textSearch: null, isUse: true);

            // Assert
            var okResult = actual as OkObjectResult;
            Assert.AreEqual(200, okResult.StatusCode);
        }
```

- Kiểm tra thất bại: Lấy dữ liệu khi không có bản ghi 
- Kiểm tra phân trang, lọc, sắp xếp 
Số lượng ước tính: 2-3 tests.


```csharp
        [TestMethod()]

        public async Task GetAll_SuccessNoRecord_Test()
        {
            // Act
            var actual = await _controller.GetAll(textSearch: "129381924", isUse: true);

            // Assert
            var okResult = actual as OkObjectResult;
            Assert.IsNotNull(actual);
            Assert.AreEqual(200, okResult.StatusCode);
        }

```
Update (PUT/PATCH)
- Kiểm tra thành công: Cập nhật dữ liệu hợp lệ.
```csharp

 [TestMethod()]

 public async Task UpdateRange_Success_Test()
 {
     // Arrange
     var req = new List<LabelUpdateReq>
  {
      new LabelUpdateReq { Name = "City A", LabelId = Guid.NewGuid().ToString(),IsUse=true },
      new LabelUpdateReq { Name = "City B", LabelId =  Guid.NewGuid().ToString(),IsUse=true }
  };

     // Act
     var actual = await _controller.Update(req);

     // Assert
     var okResult = actual as OkObjectResult;
     Assert.IsNotNull(actual);
     Assert.AreEqual(200, okResult.StatusCode);

 }
```

- Kiểm tra thất bại: Cập nhật với dữ liệu không hợp lệ.

```csharp


 [TestMethod()]

 public async Task UpdateRange_FailExistedName_Test()
 {
     // Arrange
     var req = new List<LabelUpdateReq>
  {
      new LabelUpdateReq { Name = "City A", LabelId = Guid.NewGuid().ToString(),IsUse=true },
      new LabelUpdateReq { Name = "City A", LabelId =  Guid.NewGuid().ToString(),IsUse=true }
  };

     // Act
     var actual = await _controller.Update(req);

     // Assert
     var okResult = actual as OkObjectResult;
     Assert.AreEqual(400, okResult.StatusCode);


 }
 ```

- Kiểm tra cập nhật bản ghi không tồn tại.

```csharp



```
Số lượng ước tính: 2-3 tests.
Delete (DELETE)
- Kiểm tra thành công: Xóa bản ghi tồn tại.
- Kiểm tra thất bại: Xóa bản ghi không tồn tại 

```csharp



```

