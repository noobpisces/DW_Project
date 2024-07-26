# XÂY DỰNG KHO DỮ LIỆU BÁN HÀNG CHO CÔNG TY THIẾT BỊ PHẦN CỨNG - Data Warehouse Project
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

### 1.2. Mô tả chi tiết tập dữ liệu

#### a) Tập `hardwareStore.csv`
#### b) Bốn bảng dữ liệu (`order_items`, `orders`, `customers`, `employees`) trong OT Database

### 1.3. Import file CSV vào 1 Table Store trong Database để dễ dàng sử dụng

## 2. Giới thiệu các công cụ được sử dụng

## CHƯƠNG 2: THIẾT KẾ XÂY DỰNG CƠ SỞ DỮ LIỆU TÁC NGHIỆP (OLAP)

### 2.1. Xác định các Business Process và bảng Fact

#### 2.1.1. Business Process: Sales Analysis
#### 2.1.2 Business Process: Inventory Management

### 2.2. Xây dựng các bảng Dimension

#### 2.2.1. DimDate
#### 2.2.2. DimProduct
#### 2.2.3. DimLocation
#### 2.2.4. DimWareHouse
#### 2.2.5. DimCustomer
#### 2.2.6. DimEmployee
#### 2.2.7. DimCategory

### 2.3. Star Schema (Lược đồ hình sao)

## CHƯƠNG 3: TÍCH HỢP DỮ LIỆU VÀO KHO (SSIS)

### 3.1. Import Dữ liệu vào các bảng dimension

#### 3.1.1. Date Dimension
  #### a) Load từ nguồn vào bảng `stgDate`
  #### b) Load dữ liệu từ `stgDate` vào `DimDate`
#### 3.1.2. Product Dimension
  #### a) Load từ nguồn vào bảng `stgProduct`
  #### b) Load dữ liệu từ `stgProduct` vào `DimProduct`
#### 3.1.3. Location Dimension
  #### a) Load từ nguồn vào bảng `stgLocation`
  #### b) Load dữ liệu từ `stgLocation` vào bảng `DimLocation`
#### 3.1.4. Customer Dimension
  #### a) Load dữ liệu từ nguồn vào `stgCustomer`
  #### b) Load dữ liệu từ `stgCustomer` vào bảng `DimCustomer`
#### 3.1.5. Employee Dimension
  #### a) Load dữ liệu từ nguồn vào `stgEmployee`
  #### b) Load dữ liệu từ `stgEmployee` vào bảng `DimEmployee`
#### 3.1.6. Warehouse Dimension
  #### a) Load dữ liệu từ nguồn vào `stgWarehouse`
  #### b) Load dữ liệu từ `stgWarehouse` vào `DimWarehouse`
#### 3.1.7. Category Dimension
  #### a) Load dữ liệu từ nguồn vào `stgCategory`
  #### b) Load dữ liệu từ `stgCategory` vào `DimCategory`

### 3.2. Import dữ liệu vào các bảng fact

#### 3.2.1. Fact Sales
  #### a) Load dữ liệu từ nguồn vào `stgSales`
  #### b) Load dữ liệu vào `Sales_fact`
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
