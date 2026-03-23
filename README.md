# 📦 Hardware Inventory Management System V 2.0.2

![Version](https://img.shields.io/badge/Version-2.0.0-blue)
![Django](https://img.shields.io/badge/Django-4.x%20%2F%205.x%20%2F%206.x-green)
![Python](https://img.shields.io/badge/Python-3.10%2B-yellow)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5-purple)
![License](https://img.shields.io/badge/License-Private-red)

A secure, role-based Django web application for managing hardware inventory — rentals, sales, returns, barcodes, and PDF receipts — built for small to medium hardware businesses.

---

## 🚀 Live Site
🔗 [https://hwinvapp.pythonanywhere.com](https://hwinvapp.pythonanywhere.com/)

---

## 📋 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Usage](#-usage)
- [User Roles](#-user-roles)
- [Pages & Routes](#-pages--routes)
- [Data Models](#-data-models)
- [Security](#-security)
- [Production (MySQL)](#-production-mysql)

---

## ✨ Features

- **Live Dashboard** — real-time stats, overdue rental alerts, recent rentals & sales
- **Product Management** — add, edit, delete, locate products with auto-generated barcodes
- **Rental Tracking** — full rent → confirm → receipt flow with overdue detection
- **Sales Management** — sell products with confirmation modal and auto receipt
- **PDF Receipts** — auto-generated PDF receipts for every rental and sale
- **Reports Dashboard** — revenue summaries, inventory bars, tabbed activity logs (admin only)
- **Role-Based Access** — Admin and Employee roles enforced at both UI and server level
- **Barcode Generation** — Code128 barcodes auto-generated on product creation
- **Mobile Responsive** — works on desktop, tablet, and phone via Bootstrap 5

---

## 🛠 Tech Stack

| Technology | Purpose |
|---|---|
| Django 4.x / 5.x / 6.x | Web framework |
| Python 3.10+ | Backend language |
| SQLite | Development database |
| MySQL | Production database |
| Bootstrap 5 | Frontend UI framework |
| python-barcode + Pillow | Barcode image generation |
| ReportLab | PDF receipt generation |
| widget-tweaks | Django form rendering |

---

## 📁 Project Structure

```
inventory_app/
│
├── core/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── products/
│   ├── static/
│   │   └── products/
│   │       └── logo.png
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── forms.py
│   ├── models.py
│   ├── urls.py
│   ├── utils.py
│   └── views.py
│
├── templates/
│   └── products/
│       ├── base.html
│       ├── login.html
│       ├── dashboard.html
│       ├── reports_dashboard.html
│       ├── add_product.html
│       ├── product_list.html
│       ├── rent_product.html
│       ├── return_product.html
│       ├── sell_product.html
│       └── locate_product.html
│
├── media/
│   └── barcodes/
│
├── db.sqlite3
├── manage.py
└── requirements.txt
```

---

## ⚙️ Installation

### 1. Clone or copy the project

```bash
cd /path/to/your/projects
```

### 2. Create and activate a virtual environment

```bash
python3 -m venv venv
source venv/bin/activate        # Linux / macOS
venv\Scripts\activate           # Windows
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Apply database migrations

```bash
python manage.py migrate
```

### 5. Create a superuser (Admin account)

```bash
python manage.py createsuperuser
```

### 6. Run the development server

```bash
python manage.py runserver
```

Visit: [http://127.0.0.1:8000](http://127.0.0.1:8000)

---

## 🔧 Configuration

### `core/settings.py`

```python
# Templates directory
TEMPLATES = [
    {
        'DIRS': [BASE_DIR / 'templates'],
        ...
    }
]

# Static files
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    BASE_DIR / 'products' / 'static',
]

# Media files (barcodes, uploads)
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'

# Timezone
TIME_ZONE = 'Asia/Kolkata'
```

### `requirements.txt`

```
Django>=4.0
Pillow
python-barcode
reportlab
django-widget-tweaks
mysqlclient        # only needed for MySQL/production
```

---

## 🚀 Usage

### Login
- Go to `http://127.0.0.1:8000`
- Log in with your superuser credentials (Admin) or an employee account

### Creating Employee Accounts
- Go to `http://127.0.0.1:8000/admin/`
- Create a new user — do **not** check `Superuser status` for employees

### Adding Products
- Log in as Admin → **Add Product**
- A unique 12-character Product ID and Code128 barcode are auto-generated on save

### Renting a Product
- Any logged-in user → **Rent Product**
- Fill in borrower details and return date → Confirm → Print PDF receipt

### Returning a Product
- Any logged-in user → **Return Product**
- Search by product or borrower → Confirm return → Product status resets to Available

### Selling a Product
- Admin only → **Sell Product**
- Select product, enter buyer details → Confirm → Print PDF receipt

---

## 👥 User Roles

| Page / Action | Employee | Admin |
|---|:---:|:---:|
| Dashboard | ✅ | ✅ |
| Locate Product | ✅ | ✅ |
| Rent Product | ✅ | ✅ |
| Return Product | ✅ | ✅ |
| Borrow Receipt | ✅ | ✅ |
| Add Product | ❌ | ✅ |
| Product List | ❌ | ✅ |
| Edit / Delete Product | ❌ | ✅ |
| Sell Product | ❌ | ✅ |
| Sale Receipt | ❌ | ✅ |
| Reports Dashboard | ❌ | ✅ |

> Access is enforced at the **view level** — employees receive HTTP 403 if they attempt to access admin URLs directly.

---

## 🗺 Pages & Routes

| URL | View | Access |
|---|---|---|
| `/` | Login | Public |
| `/logout/` | Logout | Authenticated |
| `/dashboard/` | Dashboard | All users |
| `/reports/` | Reports Dashboard | Admin only |
| `/add-product/` | Add Product | Admin only |
| `/products/` | Product List | Admin only |
| `/product/<pk>/detail/` | Product Detail API | All users |
| `/product/<pk>/edit/` | Edit Product | Admin only |
| `/product/<pk>/delete/` | Delete Product | Admin only |
| `/rent/` | Rent Product | All users |
| `/return/` | Return Product | All users |
| `/sell/` | Sell Product | Admin only |
| `/locate/` | Locate Product | All users |
| `/receipt/borrow/<pk>/` | Borrow Receipt PDF | All users |
| `/receipt/sale/<pk>/` | Sale Receipt PDF | Admin only |
| `/api/products/search/` | Product Search API | All users |
| `/media/barcodes/<filename>` | Secure Barcode | All users |

---

## 🗄 Data Models

### Product
| Field | Type | Notes |
|---|---|---|
| `product_id` | CharField | Auto-generated 12-char UUID |
| `name` | CharField | Product name |
| `description` | TextField | Optional |
| `container_number` | CharField | Physical location |
| `price` | DecimalField | Optional |
| `barcode_image` | ImageField | Auto-generated Code128 |
| `status` | CharField | `available` / `rented` / `sold` |
| `created_at` | DateTimeField | Auto timestamp |

### Borrow
| Field | Type | Notes |
|---|---|---|
| `product` | FK → Product | |
| `borrower_name` | CharField | |
| `borrower_phone` | CharField | |
| `borrow_date` | DateTimeField | Auto |
| `return_date` | DateField | Agreed return date |
| `returned` | BooleanField | Default False |
| `receipt_number` | CharField | Auto R-MMYY-NNNN |

### Sale
| Field | Type | Notes |
|---|---|---|
| `product` | FK → Product | |
| `buyer_name` | CharField | |
| `payment_method` | CharField | |
| `price_sold` | DecimalField | Optional |
| `sold_at` | DateTimeField | Auto |
| `receipt_number` | CharField | Auto S-MMYY-NNNN |

### Returned
| Field | Type | Notes |
|---|---|---|
| `product` | FK → Product | |
| `borrow` | FK → Borrow | |
| `borrower_name` | CharField | Snapshot |
| `return_date_agreed` | DateField | |
| `return_date_actual` | DateTimeField | |
| `borrow_receipt_number` | CharField | |

---

## 🔒 Security

- **Session Authentication** — all routes protected by `@login_required`
- **Role Checks** — `is_superuser` enforced at view level via `HttpResponseForbidden`
- **CSRF Protection** — Django's built-in CSRF middleware active on all POST forms
- **Secure Barcode Serving** — barcodes served via `@login_required` view, not exposed via `MEDIA_URL`
- **Concurrency Safety** — `transaction.atomic()` + `select_for_update()` on all status-mutating operations
- **Database Integrity** — FK constraints block deletion of products with active borrow/sale records

---

## 🗃 Production (MySQL)

### 1. Install MySQL client

```bash
pip install mysqlclient
```

### 2. Update `settings.py`

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'inventory_db',
        'USER': 'your_mysql_user',
        'PASSWORD': 'your_mysql_password',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}
```

### 3. Run migrations

```bash
python manage.py migrate
```

No code changes required — only the `DATABASES` setting needs to change.

---

## 📄 Version History

| Version | Changes |
|---|---|
| V1.0.0 | Initial release — basic product, rent, sell, locate |
| V1.0.1 | Bug fixes — race condition, N+1 query optimization |
| V2.0.0 | Returned model, reports dashboard, role enforcement, PDF receipts, barcode security, mobile UI |
| V2.0.2 | Bug fixes, Icon Updated |

---

*Hardware Inventory Management System — V2.0.2 — 2026*
