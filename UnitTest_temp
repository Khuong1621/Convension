
# Unit Test Require

## Các API CRUD cơ bản :

1. Create (POST)
2. Read (GET)
3. Update (PUT/PATCH)
4. Delete (DELETE)

## Số lượng unit test cho mỗi API

Create (POST)

- Kiểm tra thành công: Thêm mới một bản ghi hợp lệ

```csharp
[TestMethod()]
public async Task Create_Success_Test()
{
    // Arrange
    var newLabel = new LabelCreateReq
    {
        Name = "City C",
        IsUse = true
    };

    var createdLabel = new Label
    {
        LabelId = Guid.NewGuid().ToString(),
        Name = "City C",
        IsUse = true
    };



    var _controller = new LabelsController(mockService.Object);

    // Act
    var actual = await _controller.Create(newLabel);

    // Assert
    var createdResult = actual as CreatedAtActionResult;
    Assert.IsNotNull(actual);
    Assert.AreEqual(201, createdResult.StatusCode);
    var resultData = createdResult.Value as Label;
    Assert.AreEqual("City C", resultData.Name);
}
```

- Kiểm tra thất bại: Gửi dữ liệu không hợp lệ

```csharp
[TestMethod()]
public async Task Create_Fail_Test()
{
    // Arrange
    var newLabel = new LabelCreateReq
    {
        Name = "City A", // Simulate name conflict
        IsUse = true
    };

    var _controller = new LabelsController(mockService.Object);

    // Act
    var actual = await _controller.Create(newLabel);

    // Assert
    var badRequestResult = actual as BadRequestObjectResult;
    Assert.IsNotNull(actual);
    Assert.AreEqual(400, badRequestResult.StatusCode);
}

```

- Kiểm tra với dữ liệu trùng lặp

Số lượng ước tính: 2-3 tests

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

Số lượng ước tính: 2-3 tests.

- Kiểm tra phân trang, lọc, sắp xếp .(nếu có)

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

```csharp
[TestMethod()]
public async Task Delete_Success_Test()
{
    // Arrange
    var labelId = Guid.NewGuid().ToString();

    

    var _controller = new LabelsController(mockService.Object);

    // Act
    var actual = await _controller.Delete(labelId);

    // Assert
    var noContentResult = actual as NoContentResult;
    Assert.IsNotNull(actual);
    Assert.AreEqual(204, noContentResult.StatusCode);
}


```

- Kiểm tra thất bại: Xóa bản ghi không tồn tại

```csharp
[TestMethod()]
public async Task Delete_Fail_Test()
{
    // Arrange
    var labelId = Guid.NewGuid().ToString(); // Non-existent ID

   

    var _controller = new LabelsController(mockService.Object);

    // Act
    var actual = await _controller.Delete(labelId);

    // Assert
    var notFoundResult = actual as NotFoundResult;
    Assert.IsNotNull(actual);
    Assert.AreEqual(404, notFoundResult.StatusCode);
}


```
