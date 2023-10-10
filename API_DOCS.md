# Spitfire Superadmin API Documentation

## Table of Contents

1. [**GET /api/shop/endpoint**](#1-get-apishopendpoint)
2. [**PUT /api/shop/ban_vendor/{vendor_id}**](#2-put-apishopban_vendorvendor_id)
3. [**GET /api/shop/banned_vendors**](#3-get-apishopbanned_vendors)
4. [**PUT /api/shop/unban_vendor/{vendor_id}**](#4-put-apishopunban_vendorvendor_id)
5. [**PATCH /api/restore_shop/shop/{shop_id}**](#5-patch-apirestore_shopshopshop_id)
6. [**GET /api/events/shop/actions**](#6-get-apieventsshopactions)
7. [**PATCH /api/product/{id}**](#7-patch-apiproductid)
8. [**GET /api/product/download/log**](#8-get-apiproductdownloadlog)
9. [**PATCH /api/restore_product/{product_id}**](#9-patch-apirestore_productproduct_id)

---

### **1. GET /api/shop/endpoint**

- **Description:** This endpoint handles GET requests and provides a simple message indicating the success of the request.

- **cURL:**
  ```bash
  curl -X GET https://spitfire-superadmin-1.onrender.com/api/shop/endpoint
  ```

- **POSTMAN:**
    - **Method:** GET
    - **URL:** `https://spitfire-superadmin-1.onrender.com/api/shop/endpoint`

- **Response:**
  ```json
  {
      "message": "This is the shop endpoint under /api/shop/endpoint"
  }
  ```

### **2. PUT /api/shop/ban_vendor/{vendor_id}**

**Replace `{vendor_id}` with the actual UUID of the vendor.**

- **Description:** This endpoint handles PUT requests to ban a vendor by updating their shop data. If successful, it returns the details of the banned vendor.

- **cURL:**
  ```bash
  curl -X PUT https://spitfire-superadmin-1.onrender.com/api/shop/ban_vendor/{vendor_id}
  ```
  
- **POSTMAN:**
    - **Method:** PUT
    - **URL:** `https://spitfire-superadmin-1.onrender.com/api/shop/ban_vendor/{vendor_id}`
    - **Headers:** None
    - **Body:** None (Assuming the `vendor_id` is provided as a path variable)

- **Response:**
  ```json
  {
      "message": "Vendor account banned temporarily.",
      "vendor_details": {
          "id": "Vendor_ID",
          "merchant_id": "Merchant_ID",
          "name": "Vendor_Name",
          "policy_confirmation": "Policy_Confirmation",
          "restricted": "temporary",
          "admin_status": "suspended",
          "is_deleted": false,
          "reviewed": false,
          "rating": 0.0,
          "created_at": "Timestamp",
          "updated_at": "Timestamp"
      }
  }
  ```

- **Error (HTTP 400 - Vendor Already Banned):**
  ```json
  {
      "error": "Vendor is already banned."
  }
  ```

- **Error (HTTP 404 - Vendor Not Found):**
  ```json
  {
      "error": "Vendor not found."
  }
  ```

### **3. GET /api/shop/banned_vendors**

- **Description:** This endpoint handles GET requests and retrieves a list of all temporarily banned vendors.

- **cURL:**
  ```bash
  curl -X GET https://spitfire-superadmin-1.onrender.com/api/shop/banned_vendors
  ```
  
- **POSTMAN:**
    - **Method:** GET
    - **URL:** `https://spitfire-superadmin-1.onrender.com/api/shop/banned_vendors`
    - **Headers:** None
    - **Body:** None

- **Response:**
  ```json
  {
      "message": "Banned vendors retrieved successfully.",
      "banned_vendors": [
          {
              "id": "Vendor_ID",
              "merchant_id": "Merchant_ID",
              "name": "Vendor_Name",
              "policy_confirmation": "Policy_Confirmation",
              "restricted": "temporary",
              "admin_status": "suspended",
              "is_deleted": false,
              "reviewed": false,
              "rating": 0.0,
              "created_at": "Timestamp",
              "updated_at": "Timestamp"
          }
          // Additional banned vendor entries if available
      ]
  }
  ```

### **4. PUT /api/shop/unban_vendor/{vendor_id}**

**Unban a Vendor**

- **Description:** This endpoint is used to unban a vendor by updating their 'restricted' and 'admin_status' fields. The 'restricted' field is set to 'no' to indicate that the vendor is no longer restricted, and the 'admin_status' field is set to 'approved' to indicate that the vendor's status has been updated. Proper authentication and authorization checks should be added to secure this endpoint.

- **cURL:**
  ```bash
  curl -X PUT https://spitfire-superadmin-1.onrender.com/api/shop/unban_vendor/{vendor_id}
  ```

- **POSTMAN:**
    - **Method:** PUT
    - **URL:** `https://spitfire-superadmin-1.onrender.com/api/shop/unban_vendor/{vendor_id}`
    - **Headers:**
      - `Content-Type: application/json`
    - **Body:**
      ```json
      {
          "status": "no"
      }
      ```

- **Path Variable:**
    - `{vendor_id}`: Unique identifier of the vendor to be unbanned.

- **Response:**
  ```json
  {
      "status": "Success",
      "message": "Vendor unbanned successfully.",
      "vendor_details": {
          "id": "Vendor_ID",
          "merchant_id": "Merchant_ID",
          "name": "Vendor_Name",
          "policy_confirmation": "Policy_Confirmation_Status",
          "restricted": "no",
          "admin_status": "approved",
          "is_deleted": "Shop_Deletion_Status",
          "reviewed": "Review_Status",
          "rating": "Vendor_Rating",
          "created_at": "Creation_Timestamp",
          "updated_at": "Last_Update_Timestamp"
      }
  }
  ```

- **Error (HTTP 404 - Vendor Not Found):**
  ```json
  {
      "status": "Error",
      "message": "Vendor not found."
  }
  ```

- **Error (HTTP 400 - Vendor's Shop Not Active):

**
  ```json
  {
      "status": "Error",
      "message": "Vendor's shop is not active. Cannot unban."
  }
  ```

- **Error (HTTP 500 - Internal Server Error):**
  ```json
  {
      "message": "An error occurred.",
      "error": "Error_Message"
  }
  ```

### **5. PATCH /api/restore_shop/shop/{shop_id}**

**Restore a Temporarily Deleted Shop**

- **Description:** This endpoint restores a temporarily deleted shop by setting its `is_deleted` attribute from "temporary" to "active". If the shop with the provided ID is not marked as deleted, it returns a success message indicating the same.

- **cURL:**
  ```bash
  curl -X PATCH https://spitfire-superadmin-1.onrender.com/api/restore_shop/shop/{shop_id}
  ```

- **POSTMAN:**
    - **Method:** PATCH
    - **URL:** `https://spitfire-superadmin-1.onrender.com/api/restore_shop/shop/{shop_id}`
    - **Headers:**
      - `Content-Type: application/json`
    - **Body:**
      ```json
      {
          "status": "temporary"
      }
      ```

- **Path Variable:**
    - `{shop_id}`: Unique identifier of the shop to be restored.

- **Response:**
  - **Success (HTTP 200 - Shop Restored Successfully):**
    ```json
    {
        "message": "Shop restored successfully"
    }
    ```
  
  - **Success (HTTP 200 - Shop Not Marked as Deleted):**
    ```json
    {
        "message": "Shop is not marked as deleted"
    }
    ```

  - **Error (HTTP 400 - Invalid JSON Data):**
    ```json
    {
        "error": "Bad Request",
        "message": "JSON data required"
    }
    ```

  - **Error (HTTP 404 - Invalid Shop):**
    ```json
    {
        "error": "Not Found",
        "message": "Invalid shop"
    }
    ```

### **6. GET /api/events/shop/actions**

- **Description:** This endpoint handles GET requests and retrieves a list of all shop-related actions from the event logs.

- **cURL:**
  ```bash
  curl -X GET https://spitfire-superadmin-1.onrender.com/api/events/shop/actions
  ```

- **POSTMAN:**
    - **Method:** GET
    - **URL:** `https://spitfire-superadmin-1.onrender.com/api/events/shop/actions`
    - **Headers:** None
    - **Body:** None

- **Response:**
  - **Success (HTTP 200):**
    ```json
    [
        {
            "id": "Action_ID",
            "shop_id": "Shop_ID",
            "action_type": "Action_Type",
            "timestamp": "Timestamp"
        },
        // Additional shop action entries if available
    ]
    ```

  - **Error (HTTP 500 - Internal Server Error):**
    ```json
    {
        "error": "Internal Server Error",
        "message": "An error occurred while processing the request"
    }
    ```

### **7. PATCH /api/product/{id}**

**Replace `{id}` with the ID of the product.**

- **Description:** This endpoint handles PATCH requests to temporarily delete a product by updating the 'is_deleted' field of the product in the database to 'temporary'. It also logs the action in the product_logs table.

- **cURL:**
  ```bash
  curl -X PATCH https://spitfire-superadmin-1.onrender.com/api/product/{id}
  ```
  
- **POSTMAN:**
    - **Method:** PATCH
    - **URL:** `https://spitfire-superadmin-1.onrender.com/api/product/{id}`
    - **Headers:**
      - `Content-Type: application/json`
    - **Body:**
      ```json
      {
          "status": "temporary"
      }
      ```

- **Response:**
  - **Success (HTTP 200):**
    ```json
    {
        "message": "Product temporarily deleted",
        "data": null
    }
    ```

  - **Error (HTTP 404 - Invalid Product):**
    ```json
    {
        "error": "Product not found",
        "message": "Invalid product"
    }
    ```

### **8. GET /api/product/download/log**

- **Description:** This endpoint handles GET requests and allows downloading of product logs.

- **cURL:**
  ```bash
  curl -X GET https://spitfire-superadmin-1.onrender.com/api/product/download/log
  ```
  
- **POSTMAN:**
    - **Method:** GET
    - **URL:** `https://spitfire-superadmin-1.onrender.com/api/product/download/log`

### **9. PATCH /api/restore_product/{product_id}**

**Restore a Temporarily Deleted Product**

- **Description:** This endpoint restores a temporarily deleted product by setting its `is_deleted` attribute from "temporary" to "active". If the product with the provided ID is not marked as deleted, it returns a success message indicating the same.

- **

cURL:**
  ```bash
  curl -X PATCH https://spitfire-superadmin-1.onrender.com/api/restore_product/{product_id}
  ```

- **POSTMAN:**
    - **Method:** PATCH
    - **URL:** `https://spitfire-superadmin-1.onrender.com/api/restore_product/{product_id}`
    - **Headers:** None
    - **Body:** None (Assuming the `product_id` is provided as a path variable)

- **Path Variable:**
    - `{product_id}`: Unique identifier of the product to be restored.

- **Response:**
  - **Success (HTTP 200):**
    ```json
    {
        "message": "Product restored successfully"
    }
    ```

  - **Success (HTTP 200 - Product Not Marked as Deleted):**
    ```json
    {
        "message": "Product is not marked as deleted"
    }
    ```

  - **Error (HTTP 404 - Invalid Product):**
    ```json
    {
        "error": "Invalid product",
        "message": "Product not found"
    }
    ```

These examples demonstrate how to make requests to the specified endpoints and provide detailed JSON responses for each endpoint's success and error scenarios using cURL and Postman. Remember to replace the dynamic path variables (`{vendor_id}` and `{id}`) with the correct values when making requests to the respective endpoints.
