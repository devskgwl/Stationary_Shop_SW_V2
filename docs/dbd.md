# Database Design Document - Stationery Shop Management System

## Date: June 30, 2025

## 1. Introduction

यह दस्तावेज़ स्टेशनरी शॉप मैनेजमेंट सिस्टम के लिए डेटाबेस डिज़ाइन की रूपरेखा प्रस्तुत करता है, जिसे MERN स्टैक (MongoDB, Express.js, React, Node.js) का उपयोग करके विकसित किया जाएगा। डेटाबेस के रूप में MongoDB का उपयोग किया जाएगा, जो एक NoSQL, डॉक्यूमेंट-ओरिएंटेड डेटाबेस है। यह डिज़ाइन सिस्टम की सभी कार्यात्मक आवश्यकताओं को पूरा करने और भविष्य की मापनीयता (scalability) को ध्यान में रखकर तैयार किया गया है।

प्रत्येक कलेक्शन के लिए, हम निम्न प्रदान करेंगे:
* **उद्देश्य (Purpose):** कलेक्शन का क्या काम है।
* **फ़ील्ड्स (Fields):** प्रत्येक फ़ील्ड का नाम, डेटा प्रकार, विवरण, और क्या यह अनिवार्य (Mandatory) है।
* **उदाहरण डेटा (Sample Data):** कलेक्शन में डेटा कैसे दिखेगा, इसका एक नमूना।
* **संबंध (Relationships):** अन्य कलेक्शंस से इसके क्या संबंध हैं।

## 2. डेटाबेस डिज़ाइन: मुख्य कलेक्शंस और स्कीमा

### 2.1. users कलेक्शन (Users Collection)

* **उद्देश्य:** सिस्टम के सभी उपयोगकर्ता खातों (सिस्टम एडमिन, एडमिन, मैनेजर, कैशियर, आदि) का प्रबंधन करता है।
* **फ़ील्ड्स:**

| फ़ील्ड का नाम     | डेटा प्रकार | विवरण                                                | अनिवार्य |
| :--------------- | :---------- | :--------------------------------------------------- | :------ |
| `_id`            | `ObjectId`  | MongoDB द्वारा जेनरेटेड अद्वितीय ID                 | हाँ     |
| `employeeId`     | `String`    | कर्मचारी की अद्वितीय ID (जैसे 'EMP001')             | हाँ     |
| `email`          | `String`    | लॉगिन के लिए उपयोग की जाने वाली ईमेल आईडी (अद्वितीय) | हाँ     |
| `password`       | `String`    | हैश किया गया पासवर्ड                                | हाँ     |
| `firstName`      | `String`    | उपयोगकर्ता का पहला नाम                               | हाँ     |
| `lastName`       | `String`    | उपयोगकर्ता का अंतिम नाम                               | हाँ     |
| `role`           | `String`    | उपयोगकर्ता की भूमिका ('super_admin', 'admin', 'manager', 'cashier', 'stock_keeper', 'accountant') | हाँ     |
| `permissions`    | `Array`     | विशिष्ट अनुमतियों की सूची (यदि रोल के अतिरिक्त कोई विशेष अनुमति हो) | नहीं    |
| `profileImagePath` | `String`    | प्रोफाइल पिक्चर का लोकल सर्वर पाथ (जैसे `/uploads/users/emp001.jpg`) | नहीं    |
| `status`         | `String`    | उपयोगकर्ता अकाउंट का स्टेटस ('active', 'deactivated', 'pending_approval') | हाँ     |
| `loginAttempts`  | `Number`    | गलत लॉगिन प्रयासों की संख्या                         | नहीं    |
| `lastLogin`      | `Date`      | अंतिम लॉगिन की तारीख और समय                         | नहीं    |
| `createdAt`      | `Date`      | उपयोगकर्ता अकाउंट बनाने की तारीख और समय             | हाँ     |
| `updatedAt`      | `Date`      | अंतिम अपडेट की तारीख और समय                         | हाँ     |

* **उदाहरण डेटा:**

    ```json
    [
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6f1"),
        "employeeId": "SA001",
        "email": "superadmin@shop.com",
        "password": "$2a$10$abcdefghijklmnopqrstuvwxyz.hashedpassword123",
        "firstName": "Super",
        "lastName": "Admin",
        "role": "super_admin",
        "permissions": [],
        "profileImagePath": "/uploads/users/sa001.jpg",
        "status": "active",
        "loginAttempts": 0,
        "lastLogin": ISODate("2025-06-29T05:30:00.000Z"),
        "createdAt": ISODate("2025-01-15T10:00:00.000Z"),
        "updatedAt": ISODate("2025-06-29T05:30:00.000Z")
      },
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6f2"),
        "employeeId": "ADM001",
        "email": "admin@shop.com",
        "password": "$2a$10$abcdefghijklmnopqrstuvwxyz.hashedpassword456",
        "firstName": "Shop",
        "lastName": "Owner",
        "role": "admin",
        "permissions": [],
        "profileImagePath": "/uploads/users/adm001.png",
        "status": "active",
        "loginAttempts": 0,
        "lastLogin": ISODate("2025-06-29T05:45:00.000Z"),
        "createdAt": ISODate("2025-02-01T09:00:00.000Z"),
        "updatedAt": ISODate("2025-06-29T05:45:00.000Z")
      },
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6f3"),
        "employeeId": "CASH01",
        "email": "cashier1@shop.com",
        "password": "$2a$10$abcdefghijklmnopqrstuvwxyz.hashedpassword789",
        "firstName": "Priya",
        "lastName": "Sharma",
        "role": "cashier",
        "permissions": [],
        "profileImagePath": "/uploads/users/cash01.jpg",
        "status": "active",
        "loginAttempts": 0,
        "lastLogin": ISODate("2025-06-29T05:50:00.000Z"),
        "createdAt": ISODate("2025-03-10T11:00:00.000Z"),
        "updatedAt": ISODate("2025-06-29T05:50:00.000Z")
      }
    ]
    ```
* **संबंध:** अन्य कलेक्शंस के लिए `recordedBy`, `billedBy`, `createdBy` फ़ील्ड्स के माध्यम से संदर्भ (Reference)।

### 2.2. products कलेक्शन (Products Collection)

* **उद्देश्य:** दुकान में बेचे जाने वाले सभी उत्पादों और उनके विभिन्न SKU-आधारित वेरिएशंस की विस्तृत जानकारी संग्रहीत करता है।
* **फ़ील्ड्स:**

| फ़ील्ड का नाम           | डेटा प्रकार | विवरण                                                | अनिवार्य |
| :--------------------- | :---------- | :--------------------------------------------------- | :------ |
| `_id`                  | `ObjectId`  | MongoDB द्वारा जेनरेटेड अद्वितीय ID                 | हाँ     |
| `name`                 | `String`    | उत्पाद का सामान्य नाम (जैसे 'बॉल पेन', 'नोटबुक')      | हाँ     |
| `mainCategory`         | `String`    | मुख्य कैटेगरी (जैसे 'लेखन सामग्री', 'कागज उत्पाद')     | हाँ     |
| `subCategory`          | `String`    | उप-कैटेगरी (जैसे 'जेल पेन', 'A4 साइज')               | नहीं    |
| `brandId`              | `ObjectId`  | `brands` कलेक्शन से संदर्भ ID                         | हाँ     |
| `productImages`        | `Array`     | उत्पाद की सामान्य छवियों के लोकल पाथ का ऐरे (जैसे बॉक्स की छवि) | नहीं    |
| `variations`           | `Array`     | नेस्टेड डॉक्यूमेंट्स का ऐरे, प्रत्येक वेरिएशन के लिए | हाँ     |
| `variations[].sku`       | `String`    | अद्वितीय SKU (जैसे 'BP-BLUE-0.7MM') (अद्वितीय)       | हाँ     |
| `variations[].barcode`  | `String`    | कस्टम बारकोड/QR कोड (जैसे '123456789012') (अद्वितीय) | हाँ     |
| `variations[].color`    | `String`    | रंग (जैसे 'नीला', 'काला')                             | नहीं    |
| `variations[].size`     | `String`    | आकार (जैसे '0.7mm', 'A4')                             | नहीं    |
| `variations[].lineType` | `String`    | लाइनिंग टाइप (जैसे 'रूल्ड', 'अनरूल्ड')                 | नहीं    |
| `variations[].mrp`      | `Number`    | अधिकतम खुदरा मूल्य (MRP)                             | हाँ     |
| `variations[].sellingPrice` | `Number`    | वास्तविक विक्रय मूल्य                               | हाँ     |
| `variations[].currentStock` | `Number`    | सबसे छोटी इकाई में वर्तमान स्टॉक                   | हाँ     |
| `variations[].initialStock` | `Number`    | प्रारंभिक स्टॉक (सिस्टम में जोड़ने पर)              | नहीं    |
| `variations[].hasExpiry` | `Boolean`   | क्या इस वेरिएशन पर एक्सपायरी लागू होती है           | हाँ     |
| `variations[].reorderLevel` | `Number`    | पुनः-ऑर्डर स्तर                                     | नहीं    |
| `variations[].variationImageUrl` | `String`    | SKU-विशिष्ट छवि का लोकल पाथ (जैसे नीले पेन की फोटो) | नहीं    |
| `createdAt`            | `Date`      | उत्पाद एंट्री की तारीख और समय                       | हाँ     |
| `updatedAt`            | `Date`      | अंतिम अपडेट की तारीख और समय                         | हाँ     |

* **उदाहरण डेटा:**

    ```json
    [
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6f4"),
        "name": "Ball Pen",
        "mainCategory": "Writing Supplies",
        "subCategory": "Pens",
        "brandId": ObjectId("6681c0c0e5a8f2b3c4d5e6f9"), // Supra Brand
        "productImages": ["/uploads/products/ball_pen_box.jpg"],
        "variations": [
          {
            "sku": "BP-SUPRA-BLUE-0.7MM",
            "barcode": "8901000001011",
            "color": "Blue",
            "size": "0.7mm",
            "mrp": 7.00,
            "sellingPrice": 5.00,
            "currentStock": 250,
            "initialStock": 300,
            "hasExpiry": false,
            "reorderLevel": 100,
            "variationImageUrl": "/uploads/products/sku_supra_blue_pen.jpg"
          },
          {
            "sku": "BP-SUPRA-BLACK-0.7MM",
            "barcode": "8901000001028",
            "color": "Black",
            "size": "0.7mm",
            "mrp": 7.00,
            "sellingPrice": 5.00,
            "currentStock": 180,
            "initialStock": 200,
            "hasExpiry": false,
            "reorderLevel": 80,
            "variationImageUrl": "/uploads/products/sku_supra_black_pen.jpg"
          }
        ],
        "createdAt": ISODate("2025-04-01T08:00:00.000Z"),
        "updatedAt": ISODate("2025-06-29T06:00:00.000Z")
      },
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6f5"),
        "name": "Notebook",
        "mainCategory": "Paper Products",
        "subCategory": "Registers",
        "brandId": ObjectId("6681c0c0e5a8f2b3c4d5e6fa"), // Classmate Brand
        "productImages": ["/uploads/products/notebook_pack.jpg"],
        "variations": [
          {
            "sku": "NB-CLSMT-A4-R-100P",
            "barcode": "8902000002015",
            "size": "A4",
            "lineType": "Ruled",
            "mrp": 60.00,
            "sellingPrice": 55.00,
            "currentStock": 75,
            "initialStock": 100,
            "hasExpiry": false,
            "reorderLevel": 30,
            "variationImageUrl": "/uploads/products/sku_clsm_a4_ruled.jpg"
          }
        ],
        "createdAt": ISODate("2025-04-10T09:00:00.000Z"),
        "updatedAt": ISODate("2025-06-28T14:00:00.000Z")
      }
    ]
    ```
* **संबंध:** `brands` कलेक्शन से `brandId` के माध्यम से संदर्भ।

### 2.3. packingUnits कलेक्शन (PackingUnits Collection)

* **उद्देश्य:** विभिन्न पैकिंग इकाइयों और सबसे छोटी आधार इकाई के संबंध में उनके रूपांतरण कारकों को परिभाषित करता है।
* **फ़ील्ड्स:**

| फ़ील्ड का नाम    | डेटा प्रकार | विवरण                                                | अनिवार्य |
| :--------------- | :---------- | :--------------------------------------------------- | :------ |
| `_id`            | `ObjectId`  | MongoDB द्वारा जेनरेटेड अद्वितीय ID                 | हाँ     |
| `name`           | `String`    | यूनिट का नाम (जैसे 'पीस', 'पैकेट', 'कार्टन')        | हाँ     |
| `conversionFactor` | `Number`    | आधार यूनिट से रूपांतरण कारक (जैसे पैकेट के लिए 5, यदि 1 पैकेट = 5 पीस) | हाँ     |
| `baseUnitId`     | `ObjectId`  | सबसे छोटी आधार यूनिट के लिए संदर्भ ID (`_id` of 'Piece' unit) | हाँ     |
| `createdAt`      | `Date`      | एंट्री की तारीख और समय                              | हाँ     |
| `updatedAt`      | `Date`      | अंतिम अपडेट की तारीख और समय                         | हाँ     |

* **उदाहरण डेटा:**

    ```json
    [
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6f6"),
        "name": "Piece",
        "conversionFactor": 1,
        "baseUnitId": ObjectId("6681c0c0e5a8f2b3c4d5e6f6"), // Self-referencing for base unit
        "createdAt": ISODate("2025-01-01T00:00:00.000Z"),
        "updatedAt": ISODate("2025-01-01T00:00:00.000Z")
      },
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6f7"),
        "name": "Packet",
        "conversionFactor": 10,
        "baseUnitId": ObjectId("6681c0c0e5a8f2b3c4d5e6f6"), // Refers to "Piece"
        "createdAt": ISODate("2025-01-01T00:00:00.000Z"),
        "updatedAt": ISODate("2025-01-01T00:00:00.000Z")
      },
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6f8"),
        "name": "Carton",
        "conversionFactor": 100, // Assuming 1 Carton = 10 Packets = 100 Pieces
        "baseUnitId": ObjectId("6681c0c0e5a8f2b3c4d5e6f6"), // Refers to "Piece"
        "createdAt": ISODate("2025-01-01T00:00:00.000Z"),
        "updatedAt": ISODate("2025-01-01T00:00:00.000Z")
      }
    ]
    ```
* **संबंध:** यह स्वयं को `baseUnitId` के माध्यम से संदर्भित करता है। `products` कलेक्शन में, बिक्री या खरीद के दौरान पैकिंग यूनिट का संदर्भ दिया जाएगा (हालांकि सीधे `packing Units` कलेक्शन से ID स्टोर नहीं की जा रही है, बल्कि रूपांतरण कारक का उपयोग किया जा रहा है)।

### 2.4. customers कलेक्शन (Customers Collection)

* **उद्देश्य:** दुकान के सभी ग्राहकों की जानकारी संग्रहीत करता है, जिसमें व्यक्तिगत, स्कूल और कार्यालय शामिल हैं।
* **फ़ील्ड्स:**

| फ़ील्ड का नाम     | डेटा प्रकार | विवरण                                                | अनिवार्य |
| :--------------- | :---------- | :--------------------------------------------------- | :------ |
| `_id`            | `ObjectId`  | MongoDB द्वारा जेनरेटेड अद्वितीय ID                 | हाँ     |
| `customerType`   | `String`    | ग्राहक का प्रकार ('individual', 'school', 'office') | हाँ     |
| `name`           | `String`    | ग्राहक का नाम (व्यक्ति का नाम या संगठन का नाम)       | हाँ     |
| `contactNo`      | `String`    | प्राथमिक संपर्क नंबर                                  | हाँ     |
| `email`          | `String`    | ईमेल पता                                            | नहीं    |
| `address`        | `String`    | बिलिंग पता                                          | नहीं    |
| `shippingAddress` | `String`    | शिपिंग पता (यदि बिलिंग पते से अलग हो)              | नहीं    |
| `gstin`          | `String`    | GSTIN नंबर (स्कूल/ऑफिस के लिए)                     | नहीं    |
| `contactPersonName` | `String`    | संपर्क व्यक्ति का नाम (स्कूल/ऑफिस के लिए)           | नहीं    |
| `contactPersonDetails` | `String`    | संपर्क व्यक्ति का फ़ोन नंबर/ ईमेल (स्कूल/ऑफिस के लिए) | नहीं    |
| `status`         | `String`    | ग्राहक का स्टेटस ('active', 'inactive')             | हाँ     |
| `createdAt`      | `Date`      | ग्राहक एंट्री की तारीख और समय                       | हाँ     |
| `updatedAt`      | `Date`      | अंतिम अपडेट की तारीख और समय                         | हाँ     |

* **उदाहरण डेटा:**

    ```json
    [
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6fb"),
        "customerType": "individual",
        "name": "Anil Kumar",
        "contactNo": "9876543210",
        "email": "anil.k@example.com",
        "address": "123, Main Street, City, State",
        "shippingAddress": null,
        "gstin": null,
        "contactPersonName": null,
        "contactPersonDetails": null,
        "status": "active",
        "createdAt": ISODate("2025-05-01T10:00:00.000Z"),
        "updatedAt": ISODate("2025-05-01T10:00:00.000Z")
      },
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6fc"),
        "customerType": "school",
        "name": "Modern Public School",
        "contactNo": "0751-2345678",
        "email": "info@mps.edu",
        "address": "School Road, Gwalior, MP",
        "shippingAddress": "Behind Main Gate, School Road, Gwalior, MP",
        "gstin": "23AAAAA0000A1Z1",
        "contactPersonName": "Mr. Sharma",
        "contactPersonDetails": "9988776655, sharma@mps.edu",
        "status": "active",
        "createdAt": ISODate("2025-04-15T11:00:00.000Z"),
        "updatedAt": ISODate("2025-06-20T12:00:00.000Z")
      }
    ]
    ```
* **संबंध:** `sales` कलेक्शन से `customerId` के माध्यम से संदर्भ।

### 2.5. suppliers कलेक्शन (Suppliers Collection)

* **उद्देश्य:** उन सभी सप्लायर्स की जानकारी संग्रहीत करता है जिनसे दुकान उत्पाद खरीदती है।
* **फ़ील्ड्स:**

| फ़ील्ड का नाम       | डेटा प्रकार | विवरण                                                | अनिवार्य |
| :----------------- | :---------- | :--------------------------------------------------- | :------ |
| `_id`              | `ObjectId`  | MongoDB द्वारा जेनरेटेड अद्वितीय ID                 | हाँ     |
| `name`             | `String`    | सप्लायर का नाम                                      | हाँ     |
| `contactPersonName` | `String`    | प्राथमिक संपर्क व्यक्ति का नाम                       | हाँ     |
| `contactPersonNo`  | `String`    | प्राथमिक संपर्क नंबर                                  | हाँ     |
| `address`          | `String`    | पता                                                | नहीं    |
| `email`            | `String`    | ईमेल पता                                            | नहीं    |
| `gstin`            | `String`    | GSTIN नंबर                                          | नहीं    |
| `paymentTerms`     | `String`    | भुगतान शर्तें (जैसे 'नेट 30', 'तत्काल')          | नहीं    |
| `bankDetails`      | `Object`    | बैंक खाता जानकारी                                   | नहीं    |
| `bankDetails.bankName` | `String`    | बैंक का नाम                                        | नहीं    |
| `bankDetails.accountNo` | `String`    | खाता संख्या                                        | नहीं    |
| `bankDetails.ifsc` | `String`    | IFSC कोड                                            | नहीं    |
| `createdAt`        | `Date`      | सप्लायर एंट्री की तारीख और समय                     | हाँ     |
| `updatedAt`        | `Date`      | अंतिम अपडेट की तारीख और समय                         | हाँ     |

* **उदाहरण डेटा:**

    ```json
    [
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6fd"),
        "name": "Wholesale Stationery Mart",
        "contactPersonName": "Rajesh Kumar",
        "contactPersonNo": "9911223344",
        "address": "45, Industrial Area, City",
        "email": "rajesh@ws-mart.com",
        "gstin": "23AAAAA1111A1Z1",
        "paymentTerms": "Net 15",
        "bankDetails": {
          "bankName": "Axis Bank",
          "accountNo": "123456789012345",
          "ifsc": "AXIS0001234"
        },
        "createdAt": ISODate("2025-03-01T10:00:00.000Z"),
        "updatedAt": ISODate("2025-06-25T11:00:00.000Z")
      }
    ]
    ```
* **संबंध:** `purchaseOrders`, `goodsReceipts`, `supplierInvoices`, `financialTransactions` कलेक्शंस से `supplierId` के माध्यम से संदर्भ।

### 2.6. brands कलेक्शन (Brands Collection)

* **उद्देश्य:** सिस्टम में सभी ब्रांड्स की जानकारी संग्रहीत करता है।
* **फ़ील्ड्स:**

| फ़ील्ड का नाम | डेटा प्रकार | विवरण                                                | अनिवार्य |
| :------------ | :---------- | :--------------------------------------------------- | :------ |
| `_id`         | `ObjectId`  | MongoDB द्वारा जेनरेटेड अद्वितीय ID                 | हाँ     |
| `name`        | `String`    | ब्रांड का नाम (अद्वितीय)                             | हाँ     |
| `companyId`   | `ObjectId`  | `companies` कलेक्शन से संदर्भ ID                     | हाँ     |
| `createdAt`   | `Date`      | एंट्री की तारीख और समय                              | हाँ     |
| `updatedAt`   | `Date`      | अंतिम अपडेट की तारीख और समय                         | हाँ     |

* **उदाहरण डेटा:**

    ```json
    [
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6f9"),
        "name": "Supra",
        "companyId": ObjectId("6681c0c0e5a8f2b3c4d5e6fe"), // Refers to "Supra Industries"
        "createdAt": ISODate("2025-01-20T09:00:00.000Z"),
        "updatedAt": ISODate("2025-01-20T09:00:00.000Z")
      },
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6fa"),
        "name": "Classmate",
        "companyId": ObjectId("6681c0c0e5a8f2b3c4d5e6ff"), // Refers to "ITC Limited"
        "createdAt": ISODate("2025-01-20T09:00:00.000Z"),
        "updatedAt": ISODate("2025-01-20T09:00:00.000Z")
      }
    ]
    ```
* **संबंध:** `products` कलेक्शन से `brandId` के माध्यम से संदर्भ, `companies` कलेक्शन से `companyId` के माध्यम से संदर्भ।

### 2.7. companies कलेक्शन (Companies Collection)

* **उद्देश्य:** उत्पाद निर्माता कंपनियों की जानकारी संग्रहीत करता है।
* **फ़ील्ड्स:**

| फ़ील्ड का नाम | डेटा प्रकार | विवरण                                                | अनिवार्य |
| :------------ | :---------- | :--------------------------------------------------- | :------ |
| `_id`         | `ObjectId`  | MongoDB द्वारा जेनरेटेड अद्वितीय ID                 | हाँ     |
| `name`        | `String`    | कंपनी का नाम (अद्वितीय)                             | हाँ     |
| `address`     | `String`    | पता                                                | नहीं    |
| `contactInfo` | `String`    | संपर्क जानकारी (जैसे फोन, ईमेल)                     | नहीं    |
| `createdAt`   | `Date`      | एंट्री की तारीख और समय                              | हाँ     |
| `updatedAt`   | `Date`      | अंतिम अपडेट की तारीख और समय                         | हाँ     |

* **उदाहरण डेटा:**

    ```json
    [
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6fe"),
        "name": "Supra Industries",
        "address": "Delhi, India",
        "contactInfo": "supra@info.com",
        "createdAt": ISODate("2025-01-18T09:00:00.000Z"),
        "updatedAt": ISODate("2025-01-18T09:00:00.000Z")
      },
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e6ff"),
        "name": "ITC Limited",
        "address": "Kolkata, India",
        "contactInfo": "itc@info.com",
        "createdAt": ISODate("2025-01-18T09:00:00.000Z"),
        "updatedAt": ISODate("2025-01-18T09:00:00.000Z")
      }
    ]
    ```
* **संबंध:** `brands` कलेक्शन से `companyId` के माध्यम से संदर्भ।

### 2.8. sales कलेक्शन (Sales Collection)

* **उद्देश्य:** सभी बिक्री लेनदेन (बिल) का विस्तृत रिकॉर्ड रखता है।
* **फ़ील्ड्स:**

| फ़ील्ड का नाम       | डेटा प्रकार | विवरण                                                | अनिवार्य |
| :----------------- | :---------- | :--------------------------------------------------- | :------ |
| `_id`              | `ObjectId`  | MongoDB द्वारा जेनरेटेड अद्वितीय ID                 | हाँ     |
| `billNo`           | `String`    | सिस्टम-जनरेटेड अद्वितीय बिल नंबर (जैसे 'BILL-20250629-001') | हाँ     |
| `offlineBillRef`   | `String`    | ऑफलाइन बिल का संदर्भ संख्या (बुक नंबर + सीरियल)  | नहीं    |
| `billDate`         | `Date`      | बिल की तारीख और समय                                  | हाँ     |
| `customerId`       | `ObjectId`  | `customers` कलेक्शन से संदर्भ ID                     | हाँ     |
| `customerType`     | `String`    | ग्राहक का प्रकार ('walkin', 'registered', 'supply') | हाँ     |
| `items`            | `Array`     | बेचे गए प्रत्येक आइटम का विस्तृत विवरण              | हाँ     |
| `items[].productId` | `ObjectId`  | `products` कलेक्शन से उत्पाद ID (या SKU)             | हाँ     |
| `items[].skuUsed`  | `String`    | बेचे गए आइटम का SKU स्ट्रिंग                       | हाँ     |
| `items[].quantity` | `Number`    | बेची गई मात्रा (सबसे छोटी इकाई में)                  | हाँ     |
| `items[].unitPrice` | `Number`    | बिक्री के समय प्रति इकाई मूल्य                       | हाँ     |
| `items[].itemDiscount` | `Number`    | आइटम पर दिया गया अतिरिक्त डिस्काउंट                | नहीं    |
| `items[].totalItemAmount` | `Number`    | इस आइटम के लिए कुल राशि                             | हाँ     |
| `items[].batchNo`  | `String`    | बेचे गए आइटम का बैच नंबर                            | नहीं    |
| `items[].expiryDate` | `Date`      | एक्सपायरी डेट                                       | नहीं    |
| `subTotal`         | `Number`    | सभी आइटम्स का उप-कुल                                | हाँ     |
| `discount1Amount`  | `Number`    | कुल बिल पर पहला डिस्काउंट राशि                      | नहीं    |
| `discount2Amount`  | `Number`    | कुल बिल पर दूसरा डिस्काउंट राशि                     | नहीं    |
| `roundOffDiscount` | `Number`    | राउंड-ऑफ डिस्काउंट की राशि                          | नहीं    |
| `netPayableAmount` | `Number`    | ग्राहक को देय अंतिम राशि                             | हाँ     |
| `paymentMethod`    | `String`    | भुगतान मोड ('Cash', 'Card', 'UPI', 'Multiple', 'Credit') | हाँ     |
| `amountPaid`       | `Number`    | ग्राहक द्वारा भुगतान की गई राशि                      | हाँ     |
| `dueAmount`        | `Number`    | बकाया राशि (यदि क्रेडिट या आंशिक भुगतान हो)        | हाँ     |
| `billStatus`       | `String`    | बिल की स्थिति ('Paid', 'Pending', 'Partially Paid') | हाँ     |
| `remarks`          | `String`    | कोई अतिरिक्त टिप्पणी                                | नहीं    |
| `billedBy`         | `ObjectId`  | बिल बनाने वाले उपयोगकर्ता की ID (`users` से)        | हाँ     |
| `isCancelled`      | `Boolean`   | क्या बिल रद्द किया गया है                            | हाँ     |
| `pdfBillPath`      | `String`    | जेनरेट किए गए बिल की PDF कॉपी का लोकल पाथ            | हाँ     |
| `attachments`      | `Array`     | अतिरिक्त फाइलों के लोकल पाथ का ऐरे (रसीदें, स्क्रीनशॉट) | नहीं    |
| `createdAt`        | `Date`      | बिल बनने की तारीख और समय                            | हाँ     |
| `updatedAt`        | `Date`      | अंतिम अपडेट की तारीख और समय                         | हाँ     |

* **उदाहरण डेटा:**

    ```json
    [
      {
        "_id": ObjectId("6681c0c0e5a8f2b3c4d5e700"),
        "billNo": "BILL-20250629-001",
        "offlineBillRef": null,
        "billDate": ISODate("2025-06-29T06:05:00.000Z"),
        "customerId": ObjectId("6681c0c0e5a8f2b3c4d5e6fb"), // Anil Kumar
        "customerType": "individual",
        "items": [
          {
            "productId": ObjectId("6681c0c0e5a8f2b3c4d5e6f4"),
            "skuUsed": "BP-SUPRA-BLUE-0.7MM",
            "quantity": 5,
            "unitPrice": 5.00,
            "itemDiscount": 2.00, // (7.00 - 5.00) * 5 = 10.00, but here it's per piece
            "totalItemAmount": 25.00,
            "batchNo": null,
            "expiryDate": null
          }
        ],
        "subTotal": 25.00,
        "discount1Amount": 0.00,
        "discount2Amount": 0.00,
        "roundOffDiscount": 0.00,
        "netPayableAmount": 25.00,
        "paymentMethod": "Cash",
        "amountPaid": 25.00,
        "dueAmount": 0.00,
        "billStatus": "Paid",
        "remarks": null,
        "billedBy": ObjectId("6681c0c0e5a8f2b3c4d5e6f3"), // Priya Sharma (Cashier)
        "isCancelled": false,
        "pdfBillPath": "/uploads/bills/BILL-20250629-001.pdf",
        "attachments": [],
        "createdAt": ISODate("2025-06-29T06:05:00.000Z"),
        "updatedAt": ISODate("2025-06-29T06:05:00.000Z")
      }
    ]
    ```

