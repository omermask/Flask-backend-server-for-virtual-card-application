# Flask-backend-server-for-virtual-card-application
This backend API, built with Python and Flask, features a robust, centralized error handling system. It delivers clear, structured, and multilingual (English/Arabic) JSON for the virtual card backend server.


I'll provide you with a detailed explanation of all admin API endpoints with examples.

curl -X POST \
  http://localhost:5000/api/v1/auth/register \
  -H 'Content-Type: application/json' \
  -d '{
    "username": "johndoe",
    "email": "john.doe@example.com",
    "password": "securepassword123",
    "phone_number": "+1234567890"
  }'

register


curl -X POST \
  http://localhost:5000/api/v1/auth/login \
  -H 'Content-Type: application/json' \
  -d '{
    "identifier": "johndoe",
    "password": "securepassword123"
  }'



login


curl -X POST \
  http://localhost:5000/api/v1/auth/verify-otp \
  -H 'Content-Type: application/json' \
  -d '{
    "user_id": 123,
    "code": "654321"
  }'


OTP Verification



token

 {
  "message": "Login successful",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
  "refresh_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
  "user": {
    "id": 123,
    "username": "johndoe",
    "email": "john.doe@example.com",
    "phone_number": "+1234567890",
    "balance": "0.00",
    "kyc_status": "NOT_SUBMITTED",
    "role": "USER",
    "is_active": true,
    "created_at": "2023-01-01T00:00:00"
  }
}



## Admin API Endpoints - Complete Documentation


All admin endpoints are prefixed with `/api/admin`

### 1. User Management

#### Get All Users
**URL:** `GET /api/admin/user-list`
**Description:** Retrieve a list of all users with optional filtering
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Query Parameters:**
- [page](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L134-L134): Page number (default: 1)
- `per_page`: Items per page (default: 20)
- [username](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L12-L15): Filter by username
- [email](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L41-L41): Filter by email
- `phone`: Filter by phone number

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/user-list?page=1&per_page=10" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

**Example Response:**
```json
{
  "users": [
    {
      "id": 1,
      "username": "johndoe",
      "email": "john@example.com",
      "phone_number": "+1234567890",
      "phone_verified": true,
      "balance": "100.00",
      "kyc_status": "VERIFIED",
      "role": "USER",
      "is_active": true,
      "last_login_at": "2023-01-01T10:00:00",
      "last_login_ip": "127.0.0.1",
      "created_at": "2023-01-01T09:00:00",
      "fcm_token_registered": false
    }
  ],
  "pagination": {
    "page": 1,
    "per_page": 10,
    "total_pages": 1,
    "total_items": 1
  }
}
```

#### Get User by ID
**URL:** `GET /api/admin/users/<int:user_id>`
**Description:** Retrieve details of a specific user
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/users/1" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

**Example Response:**
```json
{
  "id": 1,
  "username": "johndoe",
  "email": "john@example.com",
  "phone_number": "+1234567890",
  "phone_verified": true,
  "balance": "100.00",
  "kyc_status": "VERIFIED",
  "role": "USER",
  "is_active": true,
  "last_login_at": "2023-01-01T10:00:00",
  "last_login_ip": "127.0.0.1",
  "created_at": "2023-01-01T09:00:00",
  "fcm_token_registered": false
}
```

#### Create User by Admin
**URL:** `POST /api/admin/users/create`
**Description:** Create a new user account
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "username": "newuser",
  "email": "newuser@example.com",
  "password": "securepassword123",
  "phone_number": "+1234509876",
  "role": "USER",
  "kyc_status": "NOT_SUBMITTED",
  "is_active": true,
  "phone_verified": false
}
```

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/users/create" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "username": "newuser",
    "email": "newuser@example.com",
    "password": "securepassword123",
    "phone_number": "+1234509876",
    "role": "USER",
    "kyc_status": "NOT_SUBMITTED",
    "is_active": true,
    "phone_verified": false
  }'
```

**Example Response:**
```json
{
  "message": "User created successfully by admin",
  "user": {
    "id": 2,
    "username": "newuser",
    "email": "newuser@example.com",
    "phone_number": "+1234509876",
    "phone_verified": false,
    "balance": "0.00",
    "kyc_status": "NOT_SUBMITTED",
    "role": "USER",
    "is_active": true,
    "last_login_at": null,
    "last_login_ip": null,
    "created_at": "2023-01-01T11:00:00",
    "fcm_token_registered": false
  }
}
```

#### Update User Status
**URL:** `PUT /api/admin/users/<int:user_id>/status`
**Description:** Update user status, role, or KYC status
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "role": "ADMIN",
  "kyc_status": "VERIFIED",
  "is_active": true
}
```

**Example Request:**
```bash
curl -X PUT \
  "http://localhost:5000/api/admin/users/2/status" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "role": "ADMIN",
    "kyc_status": "VERIFIED",
    "is_active": true
  }'
```

**Example Response:**
```json
{
  "message": "User status updated",
  "user": {
    "id": 2,
    "username": "newuser",
    "email": "newuser@example.com",
    "phone_number": "+1234509876",
    "phone_verified": false,
    "balance": "0.00",
    "kyc_status": "VERIFIED",
    "role": "ADMIN",
    "is_active": true,
    "last_login_at": null,
    "last_login_ip": null,
    "created_at": "2023-01-01T11:00:00",
    "fcm_token_registered": false
  }
}
```

#### Modify User Balance
**URL:** `POST /api/admin/users/<int:user_id>/balance`
**Description:** Add or deduct funds from user's wallet
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "amount": "50.00",
  "action": "CREDIT",
  "description": "Bonus for promotion"
}
```

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/users/2/balance" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "amount": "50.00",
    "action": "CREDIT",
    "description": "Bonus for promotion"
  }'
```

**Example Response:**
```json
{
  "message": "Balance updated successfully (CREDIT)",
  "new_balance": "50.00"
}
```

### 2. Card Management

#### Get All Cards
**URL:** `GET /api/admin/cards`
**Description:** Retrieve a list of all virtual cards
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Query Parameters:**
- [page](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L134-L134): Page number (default: 1)
- `per_page`: Items per page (default: 20)
- [user_id](file://c:\Users\Admin\Desktop\api\app\models\otp_model.py#L8-L8): Filter by user ID

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/cards?page=1&per_page=10" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

**Example Response:**
```json
{
  "cards": [
    {
      "id": 1,
      "user_id": 1,
      "card_name": "My Virtual Card",
      "last_four": "1234",
      "expiry_date": "12/25",
      "status": "ACTIVE",
      "balance": "50.00",
      "currency": "USD",
      "created_at": "2023-01-01T10:30:00"
    }
  ],
  "pagination": {
    "page": 1,
    "per_page": 10,
    "total_pages": 1,
    "total_items": 1
  }
}
```

#### Get Card Details
**URL:** `GET /api/admin/cards/<int:card_id>`
**Description:** Retrieve details of a specific card
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/cards/1" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

**Example Response:**
```json
{
  "id": 1,
  "user_id": 1,
  "card_name": "My Virtual Card",
  "last_four": "1234",
  "expiry_date": "12/25",
  "status": "ACTIVE",
  "balance": "50.00",
  "currency": "USD",
  "created_at": "2023-01-01T10:30:00"
}
```

#### Update Card Status
**URL:** `PUT /api/admin/cards/<int:card_id>/status`
**Description:** Update card status (ACTIVE, INACTIVE, FROZEN, TERMINATED)
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "status": "FROZEN"
}
```

**Example Request:**
```bash
curl -X PUT \
  "http://localhost:5000/api/admin/cards/1/status" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "status": "FROZEN"
  }'
```

**Example Response:**
```json
{
  "message": "Card status updated by admin to FROZEN",
  "card": {
    "id": 1,
    "user_id": 1,
    "card_name": "My Virtual Card",
    "last_four": "1234",
    "expiry_date": "12/25",
    "status": "FROZEN",
    "balance": "50.00",
    "currency": "USD",
    "created_at": "2023-01-01T10:30:00"
  }
}
```

#### Withdraw from Card
**URL:** `POST /api/admin/cards/<int:card_id>/withdraw`
**Description:** Withdraw funds directly from a card
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "amount": "25.00"
}
```

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/cards/1/withdraw" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "amount": "25.00"
  }'
```

**Example Response:**
```json
{
  "message": "Successfully withdrew 25.00 from card",
  "new_balance": "25.00",
  "transaction_id": "txn_1234567890"
}
```

#### Get Card Transaction History
**URL:** `GET /api/admin/cards/<int:card_id>/history`
**Description:** Retrieve transaction history for a specific card
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/cards/1/history" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

**Example Response:**
```json
[
  {
    "id": "txn_1234567890",
    "card_id": 1,
    "amount": "50.00",
    "type": "LOAD",
    "description": "Card load approved by admin",
    "created_at": "2023-01-01T11:00:00"
  }
]
```

### 3. Transaction Management

#### Get All Transactions
**URL:** `GET /api/admin/transactions`
**Description:** Retrieve a list of all transactions
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Query Parameters:**
- [page](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L134-L134): Page number (default: 1)
- `per_page`: Items per page (default: 20)
- [user_id](file://c:\Users\Admin\Desktop\api\app\models\otp_model.py#L8-L8): Filter by user ID
- [username](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L12-L15): Filter by username
- [status](file://c:\Users\Admin\Desktop\api\app\models\card_model.py#L28-L28): Filter by status (PENDING, COMPLETED, FAILED, CANCELLED)
- [type](file://c:\Users\Admin\Desktop\api\app\models\transaction_model.py#L28-L28): Filter by type (DEPOSIT, WITHDRAWAL, CARD_LOAD, CARD_SPEND)

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/transactions?page=1&per_page=10&type=DEPOSIT" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

**Example Response:**
```json
{
  "transactions": [
    {
      "id": "txn_1234567890",
      "user_id": 1,
      "username": "johndoe",
      "amount": "100.00",
      "fee": "2.00",
      "net_amount": "98.00",
      "type": "DEPOSIT",
      "status": "COMPLETED",
      "description": "Stripe deposit",
      "provider_txn_id": "stripe_12345",
      "related_card_id": null,
      "created_at": "2023-01-01T10:15:00",
      "completed_at": "2023-01-01T10:16:00"
    }
  ],
  "pagination": {
    "page": 1,
    "per_page": 10,
    "total_pages": 1,
    "total_items": 1
  }
}
```

#### Get Pending Withdrawals
**URL:** `GET /api/admin/transactions/withdrawals/pending`
**Description:** Retrieve all pending withdrawal requests
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/transactions/withdrawals/pending" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

**Example Response:**
```json
[
  {
    "id": "txn_0987654321",
    "user_id": 2,
    "username": "newuser",
    "amount": "50.00",
    "fee": "1.00",
    "net_amount": "49.00",
    "type": "WITHDRAWAL",
    "status": "PENDING",
    "description": "Bank transfer withdrawal",
    "provider_txn_id": null,
    "related_card_id": null,
    "created_at": "2023-01-01T11:30:00",
    "completed_at": null
  }
]
```

#### Approve Withdrawal
**URL:** `POST /api/admin/transactions/withdrawals/approve`
**Description:** Approve a pending withdrawal request
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "transaction_id": "txn_0987654321"
}
```

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/transactions/withdrawals/approve" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "transaction_id": "txn_0987654321"
  }'
```

**Example Response:**
```json
{
  "message": "Withdrawal approved",
  "transaction": {
    "id": "txn_0987654321",
    "user_id": 2,
    "username": "newuser",
    "amount": "50.00",
    "fee": "1.00",
    "net_amount": "49.00",
    "type": "WITHDRAWAL",
    "status": "COMPLETED",
    "description": "Bank transfer withdrawal",
    "provider_txn_id": null,
    "related_card_id": null,
    "created_at": "2023-01-01T11:30:00",
    "completed_at": "2023-01-01T12:00:00"
  }
}
```

#### Reject Withdrawal
**URL:** `POST /api/admin/transactions/withdrawals/reject`
**Description:** Reject a pending withdrawal request and return funds
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "transaction_id": "txn_0987654321",
  "reason": "Insufficient documentation"
}
```

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/transactions/withdrawals/reject" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "transaction_id": "txn_0987654321",
    "reason": "Insufficient documentation"
  }'
```

**Example Response:**
```json
{
  "message": "Withdrawal rejected",
  "transaction": {
    "id": "txn_0987654321",
    "user_id": 2,
    "username": "newuser",
    "amount": "50.00",
    "fee": "1.00",
    "net_amount": "49.00",
    "type": "WITHDRAWAL",
    "status": "FAILED",
    "description": "Rejected: Insufficient documentation. Original: Bank transfer withdrawal",
    "provider_txn_id": null,
    "related_card_id": null,
    "created_at": "2023-01-01T11:30:00",
    "completed_at": "2023-01-01T12:00:00"
  }
}
```

#### Get Pending Card Loads
**URL:** `GET /api/admin/transactions/card-loads/pending`
**Description:** Retrieve all pending card load requests
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/transactions/card-loads/pending" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

**Example Response:**
```json
[
  {
    "id": "txn_1122334455",
    "user_id": 1,
    "username": "johndoe",
    "amount": "100.00",
    "fee": "2.00",
    "net_amount": "98.00",
    "type": "CARD_LOAD",
    "status": "PENDING",
    "description": "Pending load to card **** 1234 (Fee: 2.00)",
    "provider_txn_id": null,
    "related_card_id": 1,
    "created_at": "2023-01-01T11:45:00",
    "completed_at": null
  }
]
```

#### Approve Card Load
**URL:** `POST /api/admin/transactions/card-loads/approve`
**Description:** Approve a pending card load request
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "transaction_id": "txn_1122334455"
}
```

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/transactions/card-loads/approve" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "transaction_id": "txn_1122334455"
  }'
```

**Example Response:**
```json
{
  "message": "Card load approved. Task is being processed in the background."
}
```

#### Reject Card Load
**URL:** `POST /api/admin/transactions/card-loads/reject`
**Description:** Reject a pending card load request and return funds
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "transaction_id": "txn_1122334455",
  "reason": "Invalid card status"
}
```

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/transactions/card-loads/reject" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "transaction_id": "txn_1122334455",
    "reason": "Invalid card status"
  }'
```

**Example Response:**
```json
{
  "message": "Card load rejected",
  "transaction": {
    "id": "txn_1122334455",
    "user_id": 1,
    "username": "johndoe",
    "amount": "100.00",
    "fee": "2.00",
    "net_amount": "98.00",
    "type": "CARD_LOAD",
    "status": "FAILED",
    "description": "Rejected: Invalid card status. Original: Pending load to card **** 1234 (Fee: 2.00)",
    "provider_txn_id": null,
    "related_card_id": 1,
    "created_at": "2023-01-01T11:45:00",
    "completed_at": "2023-01-01T12:15:00"
  }
}
```

### 4. KYC Management

#### Get KYC Requests
**URL:** `GET /api/admin/kyc/requests`
**Description:** Retrieve KYC requests with optional filtering by status
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Query Parameters:**
- [page](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L134-L134): Page number (default: 1)
- `per_page`: Items per page (default: 10)
- [status](file://c:\Users\Admin\Desktop\api\app\models\card_model.py#L28-L28): Filter by status (NOT_SUBMITTED, PENDING, VERIFIED, REJECTED) - default: PENDING

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/kyc/requests?status=PENDING" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

**Example Response:**
```json
{
  "requests": [
    {
      "user_id": 2,
      "username": "newuser",
      "email": "newuser@example.com",
      "kyc_status": "PENDING",
      "first_name": "New",
      "last_name": "User",
      "date_of_birth": "1990-01-01",
      "address_line1": "123 Main St",
      "city": "New York",
      "country": "USA",
      "document_type": "PASSPORT",
      "document_front_url": "https://storage.example.com/kyc/passport_front_123.jpg",
      "document_back_url": null,
      "selfie_url": "https://storage.example.com/kyc/selfie_123.jpg",
      "submitted_at": "2023-01-01T12:00:00",
      "reviewed_at": null,
      "rejection_reason": null
    }
  ],
  "pagination": {
    "page": 1,
    "per_page": 10,
    "total_pages": 1,
    "total_items": 1
  }
}
```

#### Get KYC Details
**URL:** `GET /api/admin/kyc/<int:user_id>`
**Description:** Retrieve KYC details for a specific user
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/kyc/2" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

**Example Response:**
```json
{
  "user_id": 2,
  "username": "newuser",
  "email": "newuser@example.com",
  "kyc_status": "PENDING",
  "first_name": "New",
  "last_name": "User",
  "date_of_birth": "1990-01-01",
  "address_line1": "123 Main St",
  "city": "New York",
  "country": "USA",
  "document_type": "PASSPORT",
  "document_front_url": "https://storage.example.com/kyc/passport_front_123.jpg",
  "document_back_url": null,
  "selfie_url": "https://storage.example.com/kyc/selfie_123.jpg",
  "submitted_at": "2023-01-01T12:00:00",
  "reviewed_at": null,
  "rejection_reason": null
}
```

#### Approve KYC
**URL:** `POST /api/admin/kyc/approve`
**Description:** Approve a pending KYC request
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "user_id": 2
}
```

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/kyc/approve" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": 2
  }'
```

**Example Response:**
```json
{
  "message": "User newuser KYC has been approved"
}
```

#### Reject KYC
**URL:** `POST /api/admin/kyc/reject`
**Description:** Reject a pending KYC request
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "user_id": 2,
  "reason": "Document not clear"
}
```

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/kyc/reject" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": 2,
    "reason": "Document not clear"
  }'
```

**Example Response:**
```json
{
  "message": "User newuser KYC has been rejected"
}
```

### 5. Ticket Management

#### Get All Tickets
**URL:** `GET /api/admin/tickets`
**Description:** Retrieve all support tickets with optional filtering
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Query Parameters:**
- [page](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L134-L134): Page number (default: 1)
- `per_page`: Items per page (default: 20)
- [status](file://c:\Users\Admin\Desktop\api\app\models\card_model.py#L28-L28): Filter by status (OPEN, PENDING_USER_REPLY, PENDING_ADMIN_REPLY, CLOSED)
- [department](file://c:\Users\Admin\Desktop\api\app\models\ticket_model.py#L25-L25): Filter by department (TECHNICAL, BILLING, GENERAL)
- [user_id](file://c:\Users\Admin\Desktop\api\app\models\card_model.py#L18-L18): Filter by user ID

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/tickets?status=OPEN" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

**Example Response:**
```json
{
  "tickets": [
    {
      "id": 1,
      "user_id": 1,
      "owner_username": "johndoe",
      "subject": "Card not working",
      "status": "OPEN",
      "department": "TECHNICAL",
      "created_at": "2023-01-01T13:00:00",
      "updated_at": "2023-01-01T13:00:00"
    }
  ],
  "pagination": {
    "page": 1,
    "per_page": 20,
    "total_pages": 1,
    "total_items": 1
  }
}
```

#### Get Ticket Details
**URL:** `GET /api/admin/tickets/<int:ticket_id>`
**Description:** Retrieve details and replies for a specific ticket
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/tickets/1" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

**Example Response:**
```json
{
  "ticket": {
    "id": 1,
    "user_id": 1,
    "owner_username": "johndoe",
    "subject": "Card not working",
    "status": "OPEN",
    "department": "TECHNICAL",
    "created_at": "2023-01-01T13:00:00",
    "updated_at": "2023-01-01T13:00:00"
  },
  "replies": [
    {
      "id": 1,
      "ticket_id": 1,
      "user_id": 1,
      "author_username": "johndoe",
      "author_role": "USER",
      "message": "My virtual card is not working when I try to make a purchase.",
      "attachment_url": null,
      "created_at": "2023-01-01T13:00:00"
    }
  ]
}
```

#### Reply to Ticket
**URL:** `POST /api/admin/tickets/<int:ticket_id>/reply`
**Description:** Reply to a support ticket
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: multipart/form-data`

**Form Data:**
- [message](file://c:\Users\Admin\Desktop\api\app\models\ticket_model.py#L42-L42): Reply message
- `attachment`: Optional file attachment

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/tickets/1/reply" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -F "message=We are investigating your issue and will get back to you shortly." \
  -F "attachment=@response.txt"
```

**Example Response:**
```json
{
  "message": "Reply added successfully",
  "reply": {
    "id": 2,
    "ticket_id": 1,
    "user_id": 3,
    "author_username": "adminuser",
    "author_role": "ADMIN",
    "message": "We are investigating your issue and will get back to you shortly.",
    "attachment_url": "https://storage.example.com/tickets/response_123.txt",
    "created_at": "2023-01-01T13:30:00"
  }
}
```

#### Update Ticket Status
**URL:** `PUT /api/admin/tickets/<int:ticket_id>/status`
**Description:** Update the status of a support ticket
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "status": "PENDING_USER_REPLY"
}
```

**Example Request:**
```bash
curl -X PUT \
  "http://localhost:5000/api/admin/tickets/1/status" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "status": "PENDING_USER_REPLY"
  }'
```

**Example Response:**
```json
{
  "message": "Ticket status updated",
  "ticket": {
    "id": 1,
    "user_id": 1,
    "owner_username": "johndoe",
    "subject": "Card not working",
    "status": "PENDING_USER_REPLY",
    "department": "TECHNICAL",
    "created_at": "2023-01-01T13:00:00",
    "updated_at": "2023-01-01T13:30:00"
  }
}
```

### 6. Dashboard Statistics

#### Get Dashboard Stats
**URL:** `GET /api/admin/dashboard/stats`
**Description:** Retrieve dashboard statistics and metrics
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/dashboard/stats" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

**Example Response:**
```json
{
  "users": {
    "total_users": 2,
    "active_users": 2,
    "online_now": 1,
    "kyc_verified": 1,
    "kyc_pending": 1,
    "kyc_not_submitted": 0
  },
  "cards": {
    "total_cards_issued": 1,
    "active_cards": 1,
    "frozen_cards": 0,
    "total_cards_balance": "50.00"
  },
  "profits": {
    "profit_today": "3.00",
    "profit_this_week": "3.00",
    "profit_this_month": "3.00",
    "profit_total": "3.00"
  },
  "support": {
    "open_tickets": 1,
    "closed_tickets": 0,
    "total_tickets": 1
  },
  "pending_actions": {
    "pending_kyc": 1,
    "pending_card_loads": 0,
    "pending_withdrawals": 0
  },
  "platform_wallet": {
    "strowallet_balance": "Not Implemented"
  }
}
```

Let me check if there are any additional admin endpoints that I haven't mentioned yet by examining the admin route files.



```toolCall::list_dir::call_99d04bf4510440e6be314787::INIT
```

I see there are some additional admin route files I haven't covered. Let me examine them to see if there are any additional endpoints:



```toolCall::read_file::call_a76fef83afdc453ba8551cab::INIT
```



```toolCall::read_file::call_12f07b97b79e4f34b40a7d58::INIT
```



```toolCall::read_file::call_8d3253d883224631bb23bf60::INIT
```



```toolCall::read_file::call_192dd9758fbd4b8d992e62a0::INIT
```

Now let me check the `__init__.py` file to see how these additional routes are registered:



```toolCall::read_file::call_5a3cb0fb0a22493780e1d41c::INIT
```

I see there are several additional admin endpoints that I haven't mentioned yet. Let me examine the remaining files to complete the documentation:



```toolCall::read_file::call_85c76ece4d4040359cadf8fa::INIT
```

Yes, there are several additional admin endpoints that I haven't mentioned yet. Let me provide you with the complete list of all admin endpoints:

## Additional Admin Endpoints Not Previously Mentioned

### 7. Advanced Reports

#### Get Dashboard Statistics
**URL:** `GET /api/admin/reports/dashboard`
**Description:** Get comprehensive dashboard statistics
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Query Parameters:**
- `days`: Number of days for statistics (default: 30)

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/reports/dashboard?days=7" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

#### Get Financial Summary
**URL:** `GET /api/admin/reports/financial-summary`
**Description:** Get detailed financial report
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Query Parameters:**
- `days`: Number of days for report (default: 30)
- `currency`: Currency filter (default: USD)

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/reports/financial-summary?days=30&currency=USD" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

#### Get User Activity Report
**URL:** `GET /api/admin/reports/user-activity`
**Description:** Get user activity analytics
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Query Parameters:**
- `days`: Number of days for report (default: 30)
- [limit](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L135-L135): Number of users to return (default: 20)

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/reports/user-activity?days=30&limit=10" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

### 8. Audit Logs

#### Get All Audit Logs
**URL:** `GET /api/admin/audit-logs`
**Description:** Get audit logs with filtering options
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`



**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/audit-logs?limit=20&page=1" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

#### Get Specific Audit Log
**URL:** `GET /api/admin/audit-logs/<int:log_id>`
**Description:** Get a specific audit log entry
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/audit-logs/123" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

#### Get User Audit Logs
**URL:** `GET /api/admin/audit-logs/user/<int:user_id>`
**Description:** Get all audit logs for a specific user
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Query Parameters:**
- [page](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L134-L134): Page number (default: 1)
- [limit](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L135-L135): Items per page (default: 50, max: 200)

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/audit-logs/user/1?limit=10" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

#### Get Failed Audit Logs
**URL:** `GET /api/admin/audit-logs/failed`
**Description:** Get all failed audit logs
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Query Parameters:**
- [page](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L134-L134): Page number (default: 1)
- [limit](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L135-L135): Items per page (default: 50, max: 200)

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/audit-logs/failed?limit=10" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

#### Cleanup Audit Logs
**URL:** `POST /api/admin/audit-logs/cleanup`
**Description:** Delete audit logs older than specified days
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "days": 90
}
```

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/audit-logs/cleanup" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "days": 90
  }'
```

#### Get Audit Statistics
**URL:** `GET /api/admin/audit-logs/stats`
**Description:** Get audit logs statistics
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/audit-logs/stats" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

### 9. Settings Management

#### Get All Settings
**URL:** `GET /api/admin/settings`
**Description:** Get all system settings
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/settings" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

#### Create Setting
**URL:** `POST /api/admin/settings`
**Description:** Create a new system setting
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "setting_key": "NEW_SETTING_KEY",
  "setting_value": "value",
  "is_public": false,
  "description": "Description of the setting"
}
```

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/settings" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "setting_key": "NEW_SETTING_KEY",
    "setting_value": "value",
    "is_public": false,
    "description": "Description of the setting"
  }'
```

#### Update Setting
**URL:** `PUT /api/admin/settings/<string:key>`
**Description:** Update an existing system setting
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "setting_value": "new_value",
  "is_public": true,
  "description": "Updated description"
}
```

**Example Request:**
```bash
curl -X PUT \
  "http://localhost:5000/api/admin/settings/NEW_SETTING_KEY" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "setting_value": "new_value",
    "is_public": true,
    "description": "Updated description"
  }'
```

#### Delete Setting
**URL:** `DELETE /api/admin/settings/<string:key>`
**Description:** Delete a system setting
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X DELETE \
  "http://localhost:5000/api/admin/settings/NEW_SETTING_KEY" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

### 10. Additional User Management

#### Get All Users (Alternative Endpoint)
**URL:** `GET /api/admin/users`
**Description:** Get all users with optional filtering and pagination
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Query Parameters:**
- [page](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L134-L134): Page number (default: 1)
- `per_page`: Items per page (default: 20)
- [role](file://c:\Users\Admin\Desktop\api\app\models\user_model.py#L34-L34): Filter by role (USER, ADMIN, SUPER_ADMIN)
- [kyc_status](file://c:\Users\Admin\Desktop\api\app\models\user_model.py#L28-L28): Filter by KYC status (NOT_SUBMITTED, PENDING, VERIFIED, REJECTED)
- [is_active](file://c:\Users\Admin\Desktop\api\app\models\user_model.py#L31-L31): Filter by active status (true/false)
- `search`: Search by username or email

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/users?per_page=10&role=USER" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

#### Get User Details
**URL:** `GET /api/admin/users/<int:user_id>`
**Description:** Get detailed information about a specific user
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/users/1" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

#### Deactivate User
**URL:** `POST /api/admin/users/<int:user_id>/deactivate`
**Description:** Deactivate a user account
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/users/2/deactivate" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

#### Activate User
**URL:** `POST /api/admin/users/<int:user_id>/activate`
**Description:** Activate a deactivated user account
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/users/2/activate" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

#### Delete User
**URL:** `DELETE /api/admin/users/<int:user_id>`
**Description:** Permanently delete a user account and associated data
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Example Request:**
```bash
curl -X DELETE \
  "http://localhost:5000/api/admin/users/2" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

#### Reset User Password
**URL:** `POST /api/admin/users/<int:user_id>/reset-password`
**Description:** Reset a user's password (admin)
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "new_password": "newsecurepassword123"
}
```

**Example Request:**
```bash
curl -X POST \
  "http://localhost:5000/api/admin/users/1/reset-password" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "new_password": "newsecurepassword123"
  }'
```

### 11. Reports

#### Get Financial Summary
**URL:** `GET /api/admin/reports/financial-summary`
**Description:** Get financial summary report
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Query Parameters:**
- `start_date`: Start date in ISO format (YYYY-MM-DD)
- `end_date`: End date in ISO format (YYYY-MM-DD)

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/reports/financial-summary?start_date=2023-01-01&end_date=2023-01-31" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

#### Get User Statement
**URL:** `GET /api/admin/reports/user-statement/<int:user_id>`
**Description:** Get or generate a user account statement
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Query Parameters:**
- [start_date](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L108-L108): Start date in ISO format (YYYY-MM-DD)
- [end_date](file://c:\Users\Admin\Desktop\api\app\core\validation_schemas.py#L109-L109): End date in ISO format (YYYY-MM-DD)
- `pdf`: Generate PDF statement (true/false)

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/reports/user-statement/1?start_date=2023-01-01&end_date=2023-01-31&pdf=false" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

#### Get Daily Activity
**URL:** `GET /api/admin/reports/daily-activity`
**Description:** Get daily transaction activity report
**Headers:** 
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

**Query Parameters:**
- `days`: Number of days to include (default: 30)

**Example Request:**
```bash
curl -X GET \
  "http://localhost:5000/api/admin/reports/daily-activity?days=7" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." \
  -H "Content-Type: application/json"
```

Now I've provided a complete list of all admin API endpoints in the system. These include all the user management, card management, transaction management, KYC management, ticket management, dashboard statistics, advanced reports, audit logs, settings management, and additional user management endpoints.
