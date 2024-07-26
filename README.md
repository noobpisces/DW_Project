![image](https://github.com/user-attachments/assets/91a3ecda-f548-4ce7-8f26-945c97fef824)# XÂY DỰNG KHO DỮ LIỆU BÁN HÀNG CHO CÔNG TY THIẾT BỊ PHẦN CỨNG - Data Warehouse Project
### (Project thực hành về thiết kết kho dữ liệu (xây theo mô hình star schema), dùng hệ quản trị cở sở dữ liệu SQL server, Visual Studio 2019/2022 tích hợp SSIS (SQL Server Integration Services) và SSAS (SQL Server Analysis Services),Power BI để trực quan hóa các thông tin lấy từ Data Warehouse)
### (Tất data đều có trong link (Chưa) [DBMS Project - Query Directory](https://github.com/noobpisces/DBMS_Project/tree/master/Database-Management-System-master/Query))
# Mục Lục

- [1. Tổng quan về tập dữ liệu](#1-tổng-quan-về-tập-dữ-liệu)
  - [1.1. Nguồn dữ liệu](#11-nguồn-dữ-liệu)
  - [1.2. Mô tả chi tiết tập dữ liệu](#12-mô-tả-chi-tiết-tập-dữ-liệu)
    - [a) Tập `hardwareStore.csv`](#a-tập-hardwarestorecsv)
    - [b) Bốn bảng dữ liệu (`order_items`, `orders`, `customers`, `employees`) trong OT Database](#b-bốn-bảng-dữ-liệu-order_items-orders-customers-employees-trong-ot-database)
  - [1.3. Import file CSV vào 1 Table Store trong Database để dễ dàng sử dụng](#13-import-file-csv-vào-1-table-store-trong-database-để-dễ-dàng-sử-dụng)

- [2. Giới thiệu các công cụ được sử dụng](#2-giới-thiệu-các-công-cụ-được-sử-dụng)

- [CHƯƠNG 2: THIẾT KẾ XÂY DỰNG CƠ SỞ DỮ LIỆU TÁC NGHIỆP (OLAP)](#chương-2-thiết-kế-xây-dựng-cơ-sở-dữ-liệu-tác-nghiệp-olap)
  - [2.1. Xác định các Business Process và bảng Fact](#21-xác-định-các-business-process-và-bảng-fact)
    - [2.1.1. Business Process: Sales Analysis](#211-business-process-sales-analysis)
    - [2.1.2. Business Process: Inventory Management](#212-business-process-inventory-management)
  - [2.2. Xây dựng các bảng Dimension](#22-xây-dựng-các-bảng-dimension)
    - [2.2.1. DimDate](#221-dimdate)
    - [2.2.2. DimProduct](#222-dimproduct)
    - [2.2.3. DimLocation](#223-dimlocation)
    - [2.2.4. DimWareHouse](#224-dimwarehouse)
    - [2.2.5. DimCustomer](#225-dimcustomer)
    - [2.2.6. DimEmployee](#226-dimemployee)
    - [2.2.7. DimCategory](#227-dimcategory)
  - [2.3. Star Schema (Lược đồ hình sao)](#23-star-schema-lược-đồ-hình-sao)

- [CHƯƠNG 3: TÍCH HỢP DỮ LIỆU VÀO KHO (SSIS)](#chương-3-tích-hợp-dữ-liệu-vào-kho-ssis)
  - [3.1. Import Dữ liệu vào các bảng dimension](#31-import-dữ-liệu-vào-các-bảng-dimension)
    - [3.1.1. Date Dimension](#311-date-dimension)
      - [a) Load từ nguồn vào bảng `stgDate`](#a-load-từ-nguồn-vào-bảng-stgdate)
      - [b) Load dữ liệu từ `stgDate` vào `DimDate`](#b-load-dữ-liệu-từ-stgdate-vào-dimdate)
    - [3.1.2. Product Dimension](#312-product-dimension)
      - [a) Load từ nguồn vào bảng `stgProduct`](#a-load-từ-nguồn-vào-bảng-stgproduct)
      - [b) Load dữ liệu từ `stgProduct` vào `DimProduct`](#b-load-dữ-liệu-từ-stgproduct-vào-dimproduct)
    - [3.1.3. Location Dimension](#313-location-dimension)
      - [a) Load từ nguồn vào bảng `stgLocation`](#a-load-từ-nguồn-vào-bảng-stglocation)
      - [b) Load dữ liệu từ `stgLocation` vào bảng `DimLocation`](#b-load-dữ-liệu-từ-stglocation-vào-bảng-dimlocation)
    - [3.1.4. Customer Dimension](#314-customer-dimension)
      - [a) Load dữ liệu từ nguồn vào `stgCustomer`](#a-load-dữ-liệu-từ-nguồn-vào-stgcustomer)
      - [b) Load dữ liệu từ `stgCustomer` vào bảng `DimCustomer`](#b-load-dữ-liệu-từ-stgcustomer-vào-bảng-dimcustomer)
    - [3.1.5. Employee Dimension](#315-employee-dimension)
      - [a) Load dữ liệu từ nguồn vào `stgEmployee`](#a-load-dữ-liệu-từ-nguồn-vào-stgemployee)
      - [b) Load dữ liệu từ `stgEmployee` vào bảng `DimEmployee`](#b-load-dữ-liệu-từ-stgemployee-vào-bảng-dimemployee)
    - [3.1.6. Warehouse Dimension](#316-warehouse-dimension)
      - [a) Load dữ liệu từ nguồn vào `stgWarehouse`](#a-load-dữ-liệu-từ-nguồn-vào-stgwarehouse)
      - [b) Load dữ liệu từ `stgWarehouse` vào `DimWarehouse`](#b-load-dữ-liệu-từ-stgwarehouse-vào-dimwarehouse)
    - [3.1.7. Category Dimension](#317-category-dimension)
      - [a) Load dữ liệu từ nguồn vào `stgCategory`](#a-load-dữ-liệu-từ-nguồn-vào-stgcategory)
      - [b) Load dữ liệu từ `stgCategory` vào `DimCategory`](#b-load-dữ-liệu-từ-stgcategory-vào-dimcategory)
  - [3.2. Import dữ liệu vào các bảng fact](#32-import-dữ-liệu-vào-các-bảng-fact)
    - [3.2.1. Fact Sales](#321-fact-sales)
      - [a) Load dữ liệu từ nguồn vào `stgSales`](#a-load-dữ-liệu-từ-nguồn-vào-stgsales)
      - [b) Load dữ liệu vào `Sales_fact`](#b-load-dữ-liệu-vào-sales_fact)
    - [3.2.2. Fact Product Inventory](#322-fact-product-inventory)
      - [a) Load dữ liệu từ nguồn vào `stgInventory`](#a-load-dữ-liệu-từ-nguồn-vào-stginventory)
      - [b) Load dữ liệu vào `Inventory_fact`](#b-load-dữ-liệu-vào-inventory_fact)

- [CHƯƠNG 4: PHÂN TÍCH DỮ LIỆU (SSAS)](#chương-4-phân-tích-dữ-liệu-ssas)
  - [4.1. Quá trình xây dựng mô hình](#41-quá-trình-xây-dựng-mô-hình)
    - [4.1.1. Tạo Data Source](#411-tạo-data-source)
    - [4.1.2. Tạo Data Source View](#412-tạo-data-source-view)
  - [4.2. Quá trình xây dựng khối Cube](#42-quá-trình-xây-dựng-khối-cube)
  - [4.3. Cấu hình Hierarchy](#43-cấu-hình-hierarchy)
    - [4.3.1. Tạo Hierarchy cho `Dim Date`](#431-tạo-hierarchy-cho-dim-date)
    - [4.3.2. Tạo Hierarchy cho `Dim Location`](#432-tạo-hierarchy-cho-dim-location)
    - [4.3.3. Tạo Hierarchy cho `Dim Category`](#433-tạo-hierarchy-cho-dim-category)
    - [4.3.4. Tạo Hierarchy cho `Dim Product`](#434-tạo-hierarchy-cho-dim-product)
    - [4.3.5. Tạo Hierarchy cho `Dim Employee`](#435-tạo-hierarchy-cho-dim-employee)
    - [4.3.6. Tạo Hierarchy cho `Dim WareHouse`](#436-tạo-hierarchy-cho-dim-warehouse)
    - [4.3.7. Tạo Hierarchy cho `Dim Customer`](#437-tạo-hierarchy-cho-dim-customer)
  - [4.4. Thực hiện phân tích dữ liệu](#44-thực-hiện-phân-tích-dữ-liệu)
    - [4.4.1. Câu hỏi: Số lượng các loại sản phẩm bán ra trong tháng/ quý/ năm.](#441-câu-hỏi-số-lượng-các-loại-sản-phẩm-bán-ra-trong-tháng-quý-năm)
      - [4.4.1.1. Sử dụng công cụ SSAS](#4411-sử-dụng-công-cụ-ssas)
      - [4.4.1.2. Sử dụng Pivot Table trong Excel](#4412-sử-dụng-pivot-table-trong-excel)
      - [4.4.1.3. Sử dụng Power BI](#4413-sử-dụng-power-bi)
    - [4.4.2. Câu hỏi: Cho biết doanh số các sản phẩm trong tháng/ quý/ năm.](#442-câu-hỏi-cho-biết-doanh-số-các-sản-phẩm-trong-tháng-quý-năm)
      - [4.4.2.1. Sử dụng công cụ SSAS](#4421-sử-dụng-công-cụ-ssas)
      - [4.4.2.2. Sử dụng Pivot Table trong Excel](#4422-sử-dụng-pivot-table-trong-excel)
      - [4.4.2.3. Sử dụng Power BI](#4423-sử-dụng-power-bi)
    - [4.4.3. Câu hỏi: Cho biết doanh số bán hàng của từng nhân viên trong tháng/ quý/ năm.](#443-câu-hỏi-cho-biết-doanh-số-bán-hàng-của-từng-nhân-viên-trong-tháng-quý-năm)
      - [4.4.3.1. Sử dụng công cụ SSAS](#4431-sử-dụng-công-cụ-ssas)
      - [4.4.3.2. Sử dụng Pivot Table trong Excel](#4432-sử-dụng-pivot-table-trong-excel)
      - [4.4.3.3. Sử dụng Power BI](#4433-sử-dụng-power-bi)
    - [4.4.4. Câu hỏi: Chi tiêu của khách hàng theo thời gian](#444-câu-hỏi-chi-tiêu-của-khách-hàng-theo-thời-gian)
      - [4.4.4.1. Sử dụng công cụ SSAS](#4441-sử-dụng-công-cụ-ssas)
      - [4.4.4.2. Sử dụng Pivot Table trong Excel](#4442-sử-dụng-pivot-table-trong-excel)
      - [4.4.4.3. Sử dụng Power BI](#4443-sử-dụng-power-bi)
    - [4.4.5. Câu hỏi: Quản lý số lượng sản phẩm phân bổ trong các kho.](#445-câu-hỏi-quản-lý-số-lượng-sản-phẩm-phân-bổ-trong-các-kho)
      - [4.4.5.1. Sử dụng công cụ SSAS](#4451-sử-dụng-công-cụ-ssas)
      - [4.4.5.2. Sử dụng Pivot Table trong Excel](#4452-sử-dụng-pivot-table-trong-excel)
      - [4.4.5.3. Sử dụng Power BI](#4453-sử-dụng-power-bi)







## 1. Tổng quan về tập dữ liệu


### 1.1. Nguồn dữ liệu
Sử dụng tập dữ liệu hardwareStore.csv lấy từ kaggle kết hợp với bốn bảng dữ liệu (order_items,orders,customers,employees )trong OT Database được lấy từ trang web Oracle Tutorial. 
̵	Đường dẫn tập csv hardwareStore.csv (Đã thêm cột date để phục vụ cho việc tạo data warehouse).
̵	Đường dẫn tải tập dữ liệu (Chỉ lấy 4 bảng order_items, orders, customers, employees) : OT Oracle Sample Database
### 1.2. Mô tả chi tiết tập dữ liệu

#### a) Tập `hardwareStore.csv`
Tập dữ liệu `hardwareStore.csv` gồm 23 cột và 408,104 dòng với các mô tả như sau:

| Tên cột             | Mô tả                                                       |
|---------------------|------------------------------------------------------------|
| Date                | Ngày kiểm kê                                                |
| CATEGORY_ID         | ID của loại sản phẩm                                        |
| CATEGORY_NAME       | Tên loại sản phẩm                                          |
| PRODUCT_ID          | ID của sản phẩm                                             |
| PRODUCT_NAME        | Tên sản phẩm                                               |
| DESCRIPTION         | Mô tả sản phẩm                                             |
| DESCRIPTION - Detail 1 | Mô tả chi tiết sản phẩm                                     |
| DESCRIPTION - Detail 2 | Mô tả chi tiết sản phẩm                                     |
| DESCRIPTION - Detail 3 | Mô tả chi tiết sản phẩm                                     |
| DESCRIPTION - Detail 4 | Mô tả chi tiết sản phẩm                                     |
| STANDARD_COST       | Giá sản xuất sản phẩm                                      |
| LIST_PRICE          | Giá niêm yết                                                |
| COUNTRY_ID          | ID của quốc gia                                             |
| REGION_ID           | ID của vùng                                                 |
| LOCATION_ID         | ID của khu vực                                              |
| WAREHOUSE_ID        | ID của kho                                                   |
| QUANTITY            | Số lượng sản phẩm                                          |
| WAREHOUSE_NAME      | Tên kho                                                     |
| ADDRESS             | Địa chỉ kho                                                 |
| POSTAL_CODE         | Mã bưu điện                                                 |
| CITY                | Thành phố                                                   |
| STATE               | Bang                                                        |
| COUNTRY_NAME        | Tên đất nước                                                |
#### b) Bốn bảng dữ liệu (`order_items`, `orders`, `customers`, `employees`) trong OT Database
#### Bảng `orders`

| Biến          | Mô tả                                                        |
|---------------|--------------------------------------------------------------|
| ORDER_ID      | Mã đơn hàng (Khóa chính)                                     |
| CUSTOMER_ID   | Mã khách hàng (Khóa ngoại trỏ đến `CUSTOMERS(CUSTOMER_ID)`)  |
| STATUS        | Tình trạng đơn hàng                                          |
| SALESMAN_ID   | Mã người bán hàng (Khóa ngoại trỏ đến `EMPLOYEES(EMPLOYEE_ID)`) |
| ORDER_DATE    | Ngày tạo đơn hàng                                            |

**Số dòng:** 105  
**Số cột:** 5

#### Bảng `order_items`

| Biến          | Mô tả                                                        |
|---------------|--------------------------------------------------------------|
| ORDER_ID      | Mã đơn hàng (Khóa ngoại trỏ đến `ORDERS(ORDER_ID)`)          |
| ITEM_ID       | Thứ tự sản phẩm trong đơn hàng                              |
| PRODUCT_ID    | Mã sản phẩm (Khóa ngoại trỏ đến `PRODUCTS(PRODUCT_ID)`)      |
| QUANTITY      | Số lượng sản phẩm được đặt trong đơn hàng                   |
| UNIT_PRICE    | Đơn giá sản phẩm                                             |

**Số dòng:** 665  
**Số cột:** 5

#### Bảng `customers`

| Biến          | Mô tả                                                        |
|---------------|--------------------------------------------------------------|
| CUSTOMER_ID   | Mã khách hàng (Khóa chính)                                   |
| NAME          | Tên khách hàng                                              |
| ADDRESS       | Địa chỉ của khách hàng                                      |
| WEBSITE       | Địa chỉ website của khách hàng                              |
| CREDIT_LIMIT  | Hạn mức tín dụng: giới hạn số tiền khách hàng được phép mua |

**Số dòng:** 319  
**Số cột:** 5

#### Bảng `employees`

| Biến          | Mô tả                                                        |
|---------------|--------------------------------------------------------------|
| EMPLOYEE_ID   | Mã nhân viên (Khóa chính)                                   |
| FIRST_NAME    | Họ nhân viên                                                |
| LAST_NAME     | Tên nhân viên                                               |
| EMAIL         | Email của nhân viên                                         |
| PHONE         | Số điện thoại của nhân viên                                 |
| HIRE_DATE     | Ngày tuyển dụng làm việc của nhân viên                      |
| MANAGER_ID    | Mã người quản lý của nhân viên                              |
| JOB_TITLE     | Vị trí công việc của nhân viên                              |

**Số dòng:** 107  
**Số cột:** 8
### 1.3. Import file CSV vào 1 Table Store trong Database để dễ dàng sử dụng
Để có thể dễ dàng kiểm soát các kiểu dữ liệu khi đưa vào Datawarehouse thì đưa dữ liệu từ file CSV vào table Store để dễ dàng kiểm soát
![image](https://github.com/user-attachments/assets/62d6361e-1aa0-48a3-93ad-70dff1e8cf43)

## CHƯƠNG 2: THIẾT KẾ XÂY DỰNG CƠ SỞ DỮ LIỆU TÁC NGHIỆP (OLAP)

### 2.1. Xác định các Business Process và bảng Fact
̵	Xây dựng Detailed Bus Matrix xác định các Business Process, bảng Fact, bảng Dim cần thiết.
 ![image](https://github.com/user-attachments/assets/afc0e412-06cf-4087-947d-d89080c9a00c)


#### 2.1.1. Business Process: Sales Analysis
̵	Các câu hỏi cụ thể được đặt ra:
+ Số lượng các loại sản phẩm bán ra trong tháng/ quý/ năm.
+ Cho biết doanh số các sản phẩm trong tháng/ quý/ năm.
+ Cho biết doanh số bán hàng của từng nhân viên trong tháng/ quý/ năm.
+ Câu hỏi: Chi tiêu của khách hàng theo thời gian.
̵	Bảng SalesFact
![image](https://github.com/user-attachments/assets/7b9f2850-dc9a-4176-aa1e-531928a4008d)

#### 2.1.2 Business Process: Inventory Management
̵	Các câu hỏi cụ thể được đặt ra:
+ Quản lý số lượng sản phẩm phân bổ trong các kho. 
̵	Bảng InventoryFact
![image](https://github.com/user-attachments/assets/11f3a632-4457-4bfd-bdd2-6760ecf25b88)

### 2.2. Xây dựng các bảng Dimension

#### 2.2.1. DimDate
![image](https://github.com/user-attachments/assets/4b61ec4f-2975-4e19-b772-3c5efecadfff)

#### 2.2.2. DimProduct
![image](https://github.com/user-attachments/assets/230aded4-0120-4299-aab9-55b7fe08a847)

#### 2.2.3. DimLocation
![image](https://github.com/user-attachments/assets/fe60cc4a-dbe7-4baf-bdea-d3b61e5f2b44)

#### 2.2.4. DimWareHouse
![image](https://github.com/user-attachments/assets/052400a0-5b0f-4fcf-8a48-d35cd5a9f51d)

#### 2.2.5. DimCustomer
![image](https://github.com/user-attachments/assets/fc27a4fd-b2fd-4adc-b893-e5d64c74cb5c)

#### 2.2.6. DimEmployee
![image](https://github.com/user-attachments/assets/0a88f564-f3a6-4aab-8870-82092d65f308)

#### 2.2.7. DimCategory
![image](https://github.com/user-attachments/assets/d2af5bdc-a051-4d60-a275-082a0b533ffe)

### 2.3. Star Schema (Lược đồ hình sao)
![image](https://github.com/user-attachments/assets/c4f6e291-2fd0-4778-abbc-afc8b9ff65df)

## CHƯƠNG 3: TÍCH HỢP DỮ LIỆU VÀO KHO (SSIS)
Ta phải Load từ SRC vào Stage
Stage các bảng Dim
 
![image](https://github.com/user-attachments/assets/033cd313-a1af-4963-817f-1c9352af668e)

Stage các bảng Fact
 ![image](https://github.com/user-attachments/assets/5274abda-d85b-430d-8137-2409411166e3)

Sau đó Load từ Stage vào các bản Dim và bảng Fact
Load các bảng Dim 
 
![image](https://github.com/user-attachments/assets/70b034d5-18b8-4cdc-a51f-0d03a9e844f6)

Load các bảng Fact
 ![image](https://github.com/user-attachments/assets/150b2609-9bb4-448d-9f3f-c98d4ce7748f)


### 3.1. Import Dữ liệu vào các bảng dimension

#### 3.1.1. Date Dimension
  #### a) Load từ nguồn vào bảng `stgDate`
  ![image](https://github.com/user-attachments/assets/ef4affb0-7423-4cf1-b632-2217c85e44e6)
SRC -HardwareStore là tập hợp ngày lấy từ bảng Store dùng để thống kê các các sản phẩm trong kho
 ![image](https://github.com/user-attachments/assets/427ab067-8c45-45a9-b024-01bfdbd1b8fb)

SRC – Order Date là tập hợp các ngày có thực hiện giao dịch mua hàng
 ![image](https://github.com/user-attachments/assets/00ef66e2-68e9-4dd7-8cf7-d77833245490)

Ta sẽ hội 2 nguồn dữ liệu này lại với nhau để có thể tạo ra stgDate chứa thời gian thống kê kho và thời gian giao dịch
 ![image](https://github.com/user-attachments/assets/bdfbb616-0f78-4f37-b7aa-fc20784623ce)
![image](https://github.com/user-attachments/assets/ac42a5ca-6cb6-4e2d-8cd2-360c258b32c5)

  #### b) Load dữ liệu từ `stgDate` vào `DimDate`
  ![image](https://github.com/user-attachments/assets/325377cf-9678-4ece-a56a-91d7eaff123f)
SRC – stgDate là dữ liệu từ bảng stgDate đã load ở trên
![image](https://github.com/user-attachments/assets/7fe3f06e-bc26-4c8a-9cca-b198a74e9df6)
Slowly Changing Dimension – DimDate load dữ liệu từ stgDate vào DimDate
![image](https://github.com/user-attachments/assets/653352f7-5e2f-4bff-a598-497bddf8ce1f)
Chọn loại SCD là Changing attribute (Update dòng)
![image](https://github.com/user-attachments/assets/09208713-feba-40e2-9ce5-e02e734fd287)

#### 3.1.2. Product Dimension
  #### a) Load từ nguồn vào bảng `stgProduct`
  ![image](https://github.com/user-attachments/assets/08c47bab-654e-4544-9f7a-fcbfd983d5d6)
SRC – HardwareStore chứa dữ liệu các Product từ bảng Store
 ![image](https://github.com/user-attachments/assets/dcd9ce41-bbdb-46e4-b7ea-48ad14c58633)


DST – stgProduct 
 ![image](https://github.com/user-attachments/assets/22714157-b973-44d3-bf19-36618b497560)
![image](https://github.com/user-attachments/assets/ccdb137f-637a-4924-b878-3a65f16449b3)

 

  #### b) Load dữ liệu từ `stgProduct` vào `DimProduct`
  ![image](https://github.com/user-attachments/assets/b4a957f1-e9af-4629-924d-64aac278a495)
SRC – stgProduct là dữ liệu từ bảng stgProduct
 ![image](https://github.com/user-attachments/assets/65c07b2f-a6c8-4de6-9745-35830c3b7ee2)

Slowly Changing Dimension – DimProduct Load từ bảng stgProduct vào DimProduct 
 ![image](https://github.com/user-attachments/assets/0bb47181-6ebe-41b6-b451-5be2f8ae0540)


Chọn SCD là Historical Attribute (Vẫn giữ lại dòng bị thay thế và thêm dòng mới)
 ![image](https://github.com/user-attachments/assets/71764cda-486d-44b8-afb0-191efc3cdc5a)




Khi một dòng trong bảng DimProduct cột RowIsCurrent sẽ có giá trị là 1
Ví dụ 1 khách hàng hiện tại đang có hạng tiêu dùng là bạc thì rowiscurent là 1
Sau đó được nâng hạng lên vàng thì sẽ thêm 1 dòng mới với mã khách hàng đó và hạng tiêu dùng là vàng với rowiscurent là 1 và sẽ sửa rowiscurent của dòng cũ thành 0 mà không xóa dòng cũ
 ![image](https://github.com/user-attachments/assets/027fe712-717f-43a6-a043-7ecf75d81efe)


#### 3.1.3. Location Dimension
  #### a) Load từ nguồn vào bảng `stgLocation`
  ![image](https://github.com/user-attachments/assets/f75d4f7f-75a0-41bc-b107-3d4a98f20e34)
SRC – HardwareStore là dữ liệu từ bảng Store
![image](https://github.com/user-attachments/assets/9b8cbbed-0b81-4f4f-a396-1c0f06089b69)
DST – stgLocation
![image](https://github.com/user-attachments/assets/187f7daa-0e50-4c24-9ef7-348641e47bc2)
![image](https://github.com/user-attachments/assets/dcca8e33-bcfd-47ff-bd74-6ebf2a7e3c9e)

  #### b) Load dữ liệu từ `stgLocation` vào bảng `DimLocation`
  ![image](https://github.com/user-attachments/assets/71ce3c66-c337-4bfd-918e-b19a5c3faf82)
SRC – stgLocation là dữ liệu bảng stgLocation
![image](https://github.com/user-attachments/assets/95a04f80-e19c-4aee-8f36-97f84d9dddf4)
Slowly Changing Dimension – DimLocation 
![image](https://github.com/user-attachments/assets/f7b328c2-49b7-4661-8a32-ed9938ae5453)
SCD chọn Historical Attibute
![image](https://github.com/user-attachments/assets/6bdb2e8a-6fbb-4e7c-a613-6b06c0180bde)
Historical attribute options chọn RowIsCurent
![image](https://github.com/user-attachments/assets/757cec78-51da-4d33-aa2d-42ad126e22f7)

#### 3.1.4. Customer Dimension
  #### a) Load dữ liệu từ nguồn vào `stgCustomer`
  ![image](https://github.com/user-attachments/assets/3fee5b1f-246e-4d4d-b31b-d4bcd6b9c777)
SRC – Customer_Data là dữ liệu từ bảng customer trong database CSV_To_Table chứa data về khách hàng
![image](https://github.com/user-attachments/assets/a51a0cf2-0c74-426d-970c-7d446745ae1a)
DST – stgCustomer 
![image](https://github.com/user-attachments/assets/cfdb7d78-8c20-4df7-9287-93c2dea11956)
![image](https://github.com/user-attachments/assets/0bb17cb0-3b99-449a-a472-b669b8306da3)


  #### b) Load dữ liệu từ `stgCustomer` vào bảng `DimCustomer`
  ![image](https://github.com/user-attachments/assets/88295a0c-4c8e-4e13-8fb6-5fa82bf3ea77)
SRC - stgCustomer chứa dữ liệu bảng stgCustomer
![image](https://github.com/user-attachments/assets/9d3177a4-814d-4529-8813-f3b97f7ba9cc)
Slowly Changing Dimension – DimCustomer load dữ liệu vào bảng DimCustomer
![image](https://github.com/user-attachments/assets/3ffe198c-50bf-4d51-83fc-5c564c8392ee)
SCD chọn Historical attribute
![image](https://github.com/user-attachments/assets/a9ab2a01-0869-4f97-a0b1-07bb172fbb67)
Historical attribute options chọn RowIsCurrent
![image](https://github.com/user-attachments/assets/24522e4e-0d0f-4ad6-bf0b-f458628af868)

#### 3.1.5. Employee Dimension
  #### a) Load dữ liệu từ nguồn vào `stgEmployee`
  ![image](https://github.com/user-attachments/assets/dee39313-4384-41b0-8bf6-26e8ce4d41db)
SRC – Employee_Data là dữ liệu từ bảng Employee trong database CSV_To_Table chứa data về nhân viên
![image](https://github.com/user-attachments/assets/15f0f9d9-9d14-4014-afeb-c6bb4e20ae1f)
DST -stgEmployee là bảng stage Employee
![image](https://github.com/user-attachments/assets/6ad3fc21-c212-42b6-80ee-4554b1e4c417)
![image](https://github.com/user-attachments/assets/617481fa-b6f6-4c1f-bc9b-5d159e8eb903)

  #### b) Load dữ liệu từ `stgEmployee` vào bảng `DimEmployee`
  ![image](https://github.com/user-attachments/assets/e48d2c24-4040-4930-b6af-c1ea1d4bb56e)
SRC – stgEmployee chứa dữ liệu bảng Employee
![image](https://github.com/user-attachments/assets/8dd3770c-b44e-4a73-9a1d-6f3dcaac3f01)
Slowly Changing Dimension – DimEmployee 
![image](https://github.com/user-attachments/assets/fdebf653-6d54-48d7-acca-83544fc172ff)
SCD chọn Historical attribute
![image](https://github.com/user-attachments/assets/de7508f9-9edd-4639-b5f9-a12f16c35b00)
Historical attribute options chọn RowIsCurrent
![image](https://github.com/user-attachments/assets/71893789-a7ec-4510-86ba-48fd41833ba5)

#### 3.1.6. Warehouse Dimension
  #### a) Load dữ liệu từ nguồn vào `stgWarehouse`
  ![image](https://github.com/user-attachments/assets/e187b16b-69f3-4ec5-a559-7914e159672f)
SRC – HardwareStore chứa dữ liệu cần lấy để load vào stgWarehouse
 
![image](https://github.com/user-attachments/assets/e1f8d47f-713c-48f5-b019-9f4d0f080b52)

DST – stgWarehouse
![image](https://github.com/user-attachments/assets/438a16f2-8d29-41e4-9015-8f9bfe2808b7)
![image](https://github.com/user-attachments/assets/a27c3f83-b59d-4dc1-a21c-6b93139c1866)

 

  #### b) Load dữ liệu từ `stgWarehouse` vào `DimWarehouse`
  ![image](https://github.com/user-attachments/assets/bffe4163-117f-4c50-95a2-ffdf4e1783e5)
SRC – stgWarehouse chứa dữ liệu của bảng stgWarehouse
 
![image](https://github.com/user-attachments/assets/d29efc06-669b-4a03-88a1-f0e9f37805b5)

Slowly Changing Dimension – DimWarehouse load dữ liệu từ stage vào DimWarehouse
 ![image](https://github.com/user-attachments/assets/2ef19c2c-e329-466d-87d3-0ba096c6d7a9)

SCD chọn Historical attribute
 ![image](https://github.com/user-attachments/assets/854895e3-ab05-469f-9e35-0d253114412e)



Historical attribute options chọn RowIsCurrent
 ![image](https://github.com/user-attachments/assets/21226c1c-7405-497d-a46a-e11209572bc6)


#### 3.1.7. Category Dimension
  #### a) Load dữ liệu từ nguồn vào `stgCategory`
  ![image](https://github.com/user-attachments/assets/dcf4bc90-2987-4104-9b1e-b0afb6789b16)


SRC – HardwareStore là dữ liệu đã được truy vấn để lấy các giá trị cần thiết cho bảng stgCategory
 ![image](https://github.com/user-attachments/assets/0401b331-c643-43bc-b39b-c33f21985128)


DST -stgCategory là bảng stgCategory
 ![image](https://github.com/user-attachments/assets/f3e898f5-1f32-4ed1-a4c6-4ab22880389d)
![image](https://github.com/user-attachments/assets/2ee04dd5-f5e2-4865-9e1c-38d4eaf61b1f)

 

  #### b) Load dữ liệu từ `stgCategory` vào `DimCategory`
![image](https://github.com/user-attachments/assets/927bc2ee-411f-4c47-aeb6-c6ee2cb4e3ff)
SRC – stgCategory
 ![image](https://github.com/user-attachments/assets/f414fc0d-86aa-4f61-a45d-025cdce42bc1)


Slowly Changing Dimension- DimCategory load dữ liệu vào bảng DimCategory
 ![image](https://github.com/user-attachments/assets/b015cb66-2815-4688-b2b2-51e68c745ec6)

SCD chọn historical attribute
 ![image](https://github.com/user-attachments/assets/3cb1abe8-022b-41fa-921e-b1304ce8e015)


Historical attributes options chọn RowIsCurrent
 ![image](https://github.com/user-attachments/assets/1dc8ee8f-e0d4-41de-95aa-cae60e1c9384)


### 3.2. Import dữ liệu vào các bảng fact

#### 3.2.1. Fact Sales
  #### a) Load dữ liệu từ nguồn vào `stgSales`
  ![image](https://github.com/user-attachments/assets/430815d7-60aa-4ee7-b381-be2c0706cddf)
SRC – Sales_Data chứa dữ liệu cần thiết để đưa vào bảng stgSales
 ![image](https://github.com/user-attachments/assets/b519f0a3-e7ab-4134-aaee-fa7daf7ffe5d)

![image](https://github.com/user-attachments/assets/0e2d6336-bdbe-4ff5-a47d-46f6bb0efc2a)


DST – stgSales
 ![image](https://github.com/user-attachments/assets/839a39bb-7990-485a-a7be-73f61dcb2867)
![image](https://github.com/user-attachments/assets/5a885a5b-0698-4cbe-9319-c33093ef3c1e)

 

  #### b) Load dữ liệu vào `Sales_fact`
  ![image](https://github.com/user-attachments/assets/79603045-0fd7-49de-ac23-1b9fa0d8f8d2)
SRC – stgSales là bảng stgSales đã load từ trước
![image](https://github.com/user-attachments/assets/3e760a92-d763-4694-a222-7ebb2b4a78b2)
Lookup DimProduct ta sẽ map hai ProductID từ hai bảng lại với nhau và lấy ra thuộc tính ProductKey trong bảng DimProduct
![image](https://github.com/user-attachments/assets/8e39f86d-809d-46de-ab50-f7b6a0d38b53)
![image](https://github.com/user-attachments/assets/bfaf4e05-fdd2-4b89-87f6-997a5d39b27e)
Lookup DimCategory ta sẽ map 2 thuộc tính CategoryId trong 2 bảng lại với nhau để lấy ra thuộc tính CategoryKey trong bảng DimCategory
![image](https://github.com/user-attachments/assets/14015b14-c090-4a3c-8932-08276545751b)
![image](https://github.com/user-attachments/assets/3ca2c8b5-98b3-4132-8b68-4616e5ecf4ed)
Lookup DimDate ta sẽ map cột order_date ở bảng stgSales với cột Date trong bảng DimDate để lấy ra DateKey
 ![image](https://github.com/user-attachments/assets/b2cd23db-41d3-4ab0-862a-9641f3cda6f3)
![image](https://github.com/user-attachments/assets/3d9685d8-48fe-431a-89a1-e75d13155c09)

 
Lookup DimCustomer ta sẽ map cột customer_id trong bảng stgSales với cột CustomerID trong bảng DimCustomer để có thể lấy ra cột CustomerKey
![image](https://github.com/user-attachments/assets/34aa606d-2a9c-4a61-ba04-1021f49f23cf)
![image](https://github.com/user-attachments/assets/3cd222f5-a00d-447e-8d07-f10ec4a515a0)


 
Lookup Dim Empployee ta sẽ map cột employee_id trong bảng stgSales với cột EmployeeID trong bảng DimEmployee
 ![image](https://github.com/user-attachments/assets/22481df6-a07e-4ed7-bbaf-c5531067f50a)

 
Aggregate Ta sẽ lấy những cột cần thiết để đưa vào bảng fact
 
Kết quả
 

#### 3.2.2. Fact Product Inventory
  #### a) Load dữ liệu từ nguồn vào `stgInventory`
  #### b) Load dữ liệu vào `Inventory_fact`

## CHƯƠNG 4: PHÂN TÍCH DỮ LIỆU (SSAS)

### 4.1. Quá trình xây dựng mô hình

#### 4.1.1. Tạo Data Source
#### 4.1.2. Tạo Data Source View

### 4.2. Quá trình xây dựng khối Cube

### 4.3. Cấu hình Hierarchy

#### 4.3.1. Tạo Hierarchy cho `Dim Date`
#### 4.3.2. Tạo Hierarchy cho `Dim Location`
#### 4.3.3. Tạo Hierarchy cho `Dim Category`
#### 4.3.4. Tạo Hierarchy cho `Dim Product`
#### 4.3.5. Tạo Hierarchy cho `Dim Employee`
#### 4.3.6. Tạo Hierarchy cho `Dim WareHouse`
#### 4.3.7. Tạo Hierarchy cho `Dim Customer`

### 4.4. Thực hiện phân tích dữ liệu

#### 4.4.1. Câu hỏi: Số lượng các loại sản phẩm bán ra trong tháng/ quý/ năm.
  #### 4.4.1.1. Sử dụng công cụ SSAS
  #### 4.4.1.2. Sử dụng Pivot Table trong Excel
  #### 4.4.1.3. Sử dụng Power BI
#### 4.4.2. Câu hỏi: Cho biết doanh số các sản phẩm trong tháng/ quý/ năm.
  #### 4.4.2.1. Sử dụng công cụ SSAS
  #### 4.4.2.2. Sử dụng Pivot Table trong Excel
  #### 4.4.2.3. Sử dụng Power BI
#### 4.4.3. Câu hỏi: Cho biết doanh số bán hàng của từng nhân viên trong tháng/ quý/ năm.
  #### 4.4.3.1. Sử dụng công cụ SSAS
  #### 4.4.3.2. Sử dụng Pivot Table trong Excel
  #### 4.4.3.3. Sử dụng Power BI
#### 4.4.4. Câu hỏi: Chi tiêu của khách hàng theo thời gian
  #### 4.4.4.1. Sử dụng công cụ SSAS
  #### 4.4.4.2. Sử dụng Pivot Table trong Excel
  #### 4.4.4.3. Sử dụng Power BI
#### 4.4.5. Câu hỏi: Quản lý số lượng sản phẩm phân bổ trong các kho.
  #### 4.4.5.1. Sử dụng công cụ SSAS
  #### 4.4.5.2. Sử dụng Pivot Table trong Excel
  #### 4.4.5.3. Sử dụng Power BI

