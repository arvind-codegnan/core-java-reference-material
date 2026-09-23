# MySQL Single-Table ER Diagram Exercises: Solution Manual

This is the answer key for `mysql-single-table-er-diagram-exercises.md`.

It contains complete MySQL `CREATE TABLE` solutions for all 10 standalone use cases, sample valid records, and commented examples that should fail when constraints are enforced.

## Compatibility

- Designed for modern MySQL 8.0.16 or later, including MySQL 8.4 and 9.x.
- Use strict SQL mode so invalid `ENUM` values are rejected.
- Every solution contains one independent table and no foreign keys.
- Run the solutions in a practice schema, not in a production database.

## Create the practice database

```sql
CREATE DATABASE IF NOT EXISTS single_table_practice
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;

USE single_table_practice;
```

---

# Solution 1: Student Academic Profile

## Create `students`

```sql
CREATE TABLE students (
    student_id INT UNSIGNED AUTO_INCREMENT,
    admission_number VARCHAR(15) NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(120) NOT NULL,
    phone VARCHAR(15) NULL,
    date_of_birth DATE NOT NULL,
    program_name VARCHAR(100) NOT NULL,
    admission_date DATE NOT NULL,
    cgpa DECIMAL(4, 2) NOT NULL,
    student_status ENUM(
        'ACTIVE',
        'GRADUATED',
        'DROPPED',
        'SUSPENDED'
    ) NOT NULL DEFAULT 'ACTIVE',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL
        DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,

    CONSTRAINT pk_students
        PRIMARY KEY (student_id),

    CONSTRAINT uq_students_admission_number
        UNIQUE (admission_number),

    CONSTRAINT uq_students_email
        UNIQUE (email),

    CONSTRAINT chk_students_cgpa
        CHECK (cgpa BETWEEN 0.00 AND 10.00)
);
```

## Valid sample records

```sql
INSERT INTO students (
    admission_number,
    first_name,
    last_name,
    email,
    phone,
    date_of_birth,
    program_name,
    admission_date,
    cgpa,
    student_status
)
VALUES
    (
        'ADM2026001', 'Ananya', 'Rao',
        'ananya.rao@example.com', '+919876540001',
        '2007-05-14', 'B.Tech Computer Science',
        '2025-08-01', 8.75, 'ACTIVE'
    ),
    (
        'ADM2026002', 'Kabir', 'Mehta',
        'kabir.mehta@example.com', NULL,
        '2006-11-22', 'B.Com Finance',
        '2024-08-01', 9.10, 'ACTIVE'
    );
```

## Constraint tests

```sql
-- Expected to fail: CGPA is greater than 10.
/*
INSERT INTO students (
    admission_number, first_name, last_name, email,
    date_of_birth, program_name, admission_date, cgpa
)
VALUES (
    'ADM2026003', 'Invalid', 'CGPA', 'invalid.cgpa@example.com',
    '2007-01-01', 'B.Sc Mathematics', '2025-08-01', 10.50
);
*/

-- Expected to fail: duplicate email.
/*
INSERT INTO students (
    admission_number, first_name, last_name, email,
    date_of_birth, program_name, admission_date, cgpa
)
VALUES (
    'ADM2026004', 'Duplicate', 'Email', 'ananya.rao@example.com',
    '2007-02-01', 'B.A. English', '2025-08-01', 7.50
);
*/
```

---

# Solution 2: Product Inventory

## Create `products`

```sql
CREATE TABLE products (
    product_id INT UNSIGNED AUTO_INCREMENT,
    sku VARCHAR(20) NOT NULL,
    product_name VARCHAR(150) NOT NULL,
    category VARCHAR(80) NOT NULL,
    brand VARCHAR(80) NULL,
    unit_price DECIMAL(12, 2) NOT NULL,
    quantity_in_stock INT UNSIGNED NOT NULL DEFAULT 0,
    reorder_level INT UNSIGNED NOT NULL DEFAULT 5,
    manufacture_date DATE NULL,
    expiry_date DATE NULL,
    product_status ENUM(
        'ACTIVE',
        'OUT_OF_STOCK',
        'DISCONTINUED'
    ) NOT NULL DEFAULT 'ACTIVE',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL
        DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,

    CONSTRAINT pk_products
        PRIMARY KEY (product_id),

    CONSTRAINT uq_products_sku
        UNIQUE (sku),

    CONSTRAINT chk_products_unit_price
        CHECK (unit_price > 0),

    CONSTRAINT chk_products_quantity
        CHECK (quantity_in_stock >= 0),

    CONSTRAINT chk_products_reorder_level
        CHECK (reorder_level >= 0),

    CONSTRAINT chk_products_dates
        CHECK (
            expiry_date IS NULL
            OR manufacture_date IS NULL
            OR expiry_date >= manufacture_date
        )
);
```

## Valid sample records

```sql
INSERT INTO products (
    sku,
    product_name,
    category,
    brand,
    unit_price,
    quantity_in_stock,
    reorder_level,
    manufacture_date,
    expiry_date,
    product_status
)
VALUES
    (
        'ELEC-1001', 'Wireless Keyboard', 'Electronics',
        'KeyPro', 2499.00, 35, 10,
        '2026-01-15', NULL, 'ACTIVE'
    ),
    (
        'FOOD-2001', 'Organic Oats 1 kg', 'Groceries',
        'HealthyGrain', 320.00, 80, 20,
        '2026-07-01', '2027-01-01', 'ACTIVE'
    );
```

## Constraint tests

```sql
-- Expected to fail: price must be greater than zero.
/*
INSERT INTO products (
    sku, product_name, category, unit_price
)
VALUES (
    'BAD-PRICE', 'Invalid Product', 'Testing', -10.00
);
*/

-- Expected to fail: expiry precedes manufacture date.
/*
INSERT INTO products (
    sku, product_name, category, unit_price,
    manufacture_date, expiry_date
)
VALUES (
    'BAD-DATE', 'Invalid Dates', 'Testing', 100.00,
    '2026-06-01', '2026-05-01'
);
*/
```

---

# Solution 3: Customer Profile

## Create `customers`

```sql
CREATE TABLE customers (
    customer_id BIGINT UNSIGNED AUTO_INCREMENT,
    customer_code VARCHAR(12) NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(120) NOT NULL,
    phone VARCHAR(15) NULL,
    date_of_birth DATE NULL,
    city VARCHAR(80) NOT NULL,
    state VARCHAR(80) NOT NULL,
    postal_code VARCHAR(12) NOT NULL,
    customer_type ENUM(
        'REGULAR',
        'PREMIUM',
        'CORPORATE'
    ) NOT NULL DEFAULT 'REGULAR',
    credit_limit DECIMAL(12, 2) NOT NULL DEFAULT 0.00,
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    registered_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT pk_customers
        PRIMARY KEY (customer_id),

    CONSTRAINT uq_customers_customer_code
        UNIQUE (customer_code),

    CONSTRAINT uq_customers_email
        UNIQUE (email),

    CONSTRAINT uq_customers_phone
        UNIQUE (phone),

    CONSTRAINT chk_customers_credit_limit
        CHECK (credit_limit >= 0)
);
```

## Valid sample records

```sql
INSERT INTO customers (
    customer_code,
    first_name,
    last_name,
    email,
    phone,
    date_of_birth,
    city,
    state,
    postal_code,
    customer_type,
    credit_limit
)
VALUES
    (
        'CUS000001', 'Ishita', 'Nair',
        'ishita.nair@example.com', '+919876540101',
        '1995-04-18', 'Chennai', 'Tamil Nadu', '600001',
        'PREMIUM', 50000.00
    ),
    (
        'CUS000002', 'Manav', 'Shah',
        'manav.shah@example.com', NULL,
        NULL, 'Ahmedabad', 'Gujarat', '380001',
        'REGULAR', 0.00
    );
```

## Constraint tests

```sql
-- This is valid: another NULL phone value is allowed.
INSERT INTO customers (
    customer_code, first_name, last_name, email,
    phone, city, state, postal_code
)
VALUES (
    'CUS000003', 'Reyansh', 'Kumar', 'reyansh.kumar@example.com',
    NULL, 'Patna', 'Bihar', '800001'
);

-- Expected to fail: duplicate non-null phone number.
/*
INSERT INTO customers (
    customer_code, first_name, last_name, email,
    phone, city, state, postal_code
)
VALUES (
    'CUS000004', 'Duplicate', 'Phone', 'duplicate.phone@example.com',
    '+919876540101', 'Pune', 'Maharashtra', '411001'
);
*/

-- Expected to fail: negative credit limit.
/*
INSERT INTO customers (
    customer_code, first_name, last_name, email,
    city, state, postal_code, credit_limit
)
VALUES (
    'CUS000005', 'Invalid', 'Credit', 'invalid.credit@example.com',
    'Delhi', 'Delhi', '110001', -1000.00
);
*/
```

---

# Solution 4: Book Catalogue

## Create `books`

```sql
CREATE TABLE books (
    book_id BIGINT UNSIGNED AUTO_INCREMENT,
    isbn CHAR(13) NOT NULL,
    title VARCHAR(200) NOT NULL,
    author_name VARCHAR(120) NOT NULL,
    genre VARCHAR(60) NOT NULL,
    publisher VARCHAR(120) NULL,
    publication_year SMALLINT UNSIGNED NOT NULL,
    page_count SMALLINT UNSIGNED NOT NULL,
    book_format ENUM(
        'HARDCOVER',
        'PAPERBACK',
        'EBOOK'
    ) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    copies_available INT UNSIGNED NOT NULL,
    language VARCHAR(40) NOT NULL DEFAULT 'English',
    added_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT pk_books
        PRIMARY KEY (book_id),

    CONSTRAINT uq_books_isbn
        UNIQUE (isbn),

    CONSTRAINT chk_books_isbn_length
        CHECK (CHAR_LENGTH(isbn) = 13),

    CONSTRAINT chk_books_publication_year
        CHECK (publication_year BETWEEN 1000 AND 2100),

    CONSTRAINT chk_books_page_count
        CHECK (page_count > 0),

    CONSTRAINT chk_books_price
        CHECK (price >= 0),

    CONSTRAINT chk_books_copies_available
        CHECK (copies_available >= 0)
);
```

## Valid sample records

```sql
INSERT INTO books (
    isbn,
    title,
    author_name,
    genre,
    publisher,
    publication_year,
    page_count,
    book_format,
    price,
    copies_available,
    language
)
VALUES
    (
        '9780134685991', 'Effective Java', 'Joshua Bloch',
        'Programming', 'Addison-Wesley', 2018, 416,
        'PAPERBACK', 4999.00, 12, 'English'
    ),
    (
        '9781492078005', 'Learning SQL', 'Alan Beaulieu',
        'Database', 'O Reilly Media', 2020, 380,
        'EBOOK', 2999.00, 100, 'English'
    );
```

## Constraint tests

```sql
-- Expected to fail: page count is zero.
/*
INSERT INTO books (
    isbn, title, author_name, genre,
    publication_year, page_count, book_format,
    price, copies_available
)
VALUES (
    '9780000000001', 'Invalid Pages', 'Test Author', 'Testing',
    2026, 0, 'PAPERBACK', 500.00, 1
);
*/

-- Expected to fail: publication year is outside the permitted range.
/*
INSERT INTO books (
    isbn, title, author_name, genre,
    publication_year, page_count, book_format,
    price, copies_available
)
VALUES (
    '9780000000002', 'Invalid Year', 'Test Author', 'Testing',
    999, 100, 'HARDCOVER', 500.00, 1
);
*/
```

---

# Solution 5: Patient Registration

## Create `patients`

```sql
CREATE TABLE patients (
    patient_id BIGINT UNSIGNED AUTO_INCREMENT,
    patient_number VARCHAR(15) NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    date_of_birth DATE NOT NULL,
    biological_sex ENUM(
        'FEMALE',
        'MALE',
        'INTERSEX',
        'NOT_DISCLOSED'
    ) NOT NULL,
    blood_group ENUM(
        'A+', 'A-',
        'B+', 'B-',
        'AB+', 'AB-',
        'O+', 'O-'
    ) NULL,
    phone VARCHAR(15) NOT NULL,
    email VARCHAR(120) NULL,
    emergency_contact_name VARCHAR(100) NOT NULL,
    emergency_contact_phone VARCHAR(15) NOT NULL,
    allergies TEXT NULL,
    patient_status ENUM(
        'ACTIVE',
        'INACTIVE',
        'DECEASED'
    ) NOT NULL DEFAULT 'ACTIVE',
    registered_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT pk_patients
        PRIMARY KEY (patient_id),

    CONSTRAINT uq_patients_patient_number
        UNIQUE (patient_number)
);
```

## Valid fictional sample records

```sql
INSERT INTO patients (
    patient_number,
    first_name,
    last_name,
    date_of_birth,
    biological_sex,
    blood_group,
    phone,
    email,
    emergency_contact_name,
    emergency_contact_phone,
    allergies
)
VALUES
    (
        'PAT000001', 'Aarav', 'Test', '1990-03-15',
        'MALE', 'O+', '+919876540201',
        'aarav.test@example.com', 'Mira Test',
        '+919876540202', 'Penicillin'
    ),
    (
        'PAT000002', 'Diya', 'Sample', '1988-10-09',
        'FEMALE', NULL, '+919876540203',
        NULL, 'Rohan Sample', '+919876540204', NULL
    );
```

## Constraint tests

```sql
-- Expected to fail in strict mode: unsupported blood group.
/*
INSERT INTO patients (
    patient_number, first_name, last_name, date_of_birth,
    biological_sex, blood_group, phone,
    emergency_contact_name, emergency_contact_phone
)
VALUES (
    'PAT000003', 'Invalid', 'BloodGroup', '2000-01-01',
    'NOT_DISCLOSED', 'X+', '+919876540205',
    'Emergency Person', '+919876540206'
);
*/

-- Expected to fail: duplicate patient number.
/*
INSERT INTO patients (
    patient_number, first_name, last_name, date_of_birth,
    biological_sex, phone,
    emergency_contact_name, emergency_contact_phone
)
VALUES (
    'PAT000001', 'Duplicate', 'Number', '2000-01-01',
    'NOT_DISCLOSED', '+919876540207',
    'Emergency Person', '+919876540208'
);
*/
```

---

# Solution 6: Bank Account Summary

## Create `bank_accounts`

```sql
CREATE TABLE bank_accounts (
    account_id BIGINT UNSIGNED AUTO_INCREMENT,
    account_number CHAR(12) NOT NULL,
    account_holder_name VARCHAR(120) NOT NULL,
    account_type ENUM(
        'SAVINGS',
        'CURRENT',
        'FIXED_DEPOSIT'
    ) NOT NULL,
    balance DECIMAL(15, 2) NOT NULL DEFAULT 0.00,
    currency_code CHAR(3) NOT NULL DEFAULT 'INR',
    branch_name VARCHAR(100) NOT NULL,
    opened_date DATE NOT NULL,
    interest_rate DECIMAL(5, 2) NOT NULL DEFAULT 0.00,
    overdraft_limit DECIMAL(12, 2) NOT NULL DEFAULT 0.00,
    account_status ENUM(
        'ACTIVE',
        'FROZEN',
        'DORMANT',
        'CLOSED'
    ) NOT NULL DEFAULT 'ACTIVE',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL
        DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,

    CONSTRAINT pk_bank_accounts
        PRIMARY KEY (account_id),

    CONSTRAINT uq_bank_accounts_account_number
        UNIQUE (account_number),

    CONSTRAINT chk_bank_accounts_number_length
        CHECK (CHAR_LENGTH(account_number) = 12),

    CONSTRAINT chk_bank_accounts_balance
        CHECK (balance >= 0),

    CONSTRAINT chk_bank_accounts_interest_rate
        CHECK (interest_rate BETWEEN 0.00 AND 100.00),

    CONSTRAINT chk_bank_accounts_overdraft
        CHECK (overdraft_limit >= 0)
);
```

## Valid sample records

```sql
INSERT INTO bank_accounts (
    account_number,
    account_holder_name,
    account_type,
    balance,
    branch_name,
    opened_date,
    interest_rate,
    overdraft_limit
)
VALUES
    (
        '100000000001', 'Nisha Verma', 'SAVINGS',
        125000.00, 'Chennai Main', '2022-04-10',
        3.50, 0.00
    ),
    (
        '100000000002', 'Arun Traders', 'CURRENT',
        500000.00, 'Bengaluru Central', '2021-08-20',
        0.00, 100000.00
    );
```

## Constraint tests

```sql
-- Expected to fail: negative balance.
/*
INSERT INTO bank_accounts (
    account_number, account_holder_name, account_type,
    balance, branch_name, opened_date
)
VALUES (
    '100000000003', 'Invalid Balance', 'SAVINGS',
    -500.00, 'Test Branch', '2026-01-01'
);
*/

-- Expected to fail: interest rate is greater than 100.
/*
INSERT INTO bank_accounts (
    account_number, account_holder_name, account_type,
    balance, branch_name, opened_date, interest_rate
)
VALUES (
    '100000000004', 'Invalid Interest', 'FIXED_DEPOSIT',
    10000.00, 'Test Branch', '2026-01-01', 120.00
);
*/
```

---

# Solution 7: Vehicle Registry

## Create `vehicles`

```sql
CREATE TABLE vehicles (
    vehicle_id BIGINT UNSIGNED AUTO_INCREMENT,
    registration_number VARCHAR(20) NOT NULL,
    owner_name VARCHAR(120) NOT NULL,
    manufacturer VARCHAR(80) NOT NULL,
    model VARCHAR(80) NOT NULL,
    vehicle_type ENUM(
        'CAR',
        'MOTORCYCLE',
        'TRUCK',
        'VAN',
        'BUS'
    ) NOT NULL,
    fuel_type ENUM(
        'PETROL',
        'DIESEL',
        'ELECTRIC',
        'HYBRID',
        'CNG'
    ) NOT NULL,
    manufacture_year YEAR NOT NULL,
    purchase_date DATE NULL,
    color VARCHAR(40) NOT NULL,
    odometer_km INT UNSIGNED NOT NULL DEFAULT 0,
    insurance_expiry DATE NULL,
    vehicle_status ENUM(
        'ACTIVE',
        'IN_SERVICE',
        'SOLD',
        'SCRAPPED'
    ) NOT NULL DEFAULT 'ACTIVE',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT pk_vehicles
        PRIMARY KEY (vehicle_id),

    CONSTRAINT uq_vehicles_registration_number
        UNIQUE (registration_number),

    CONSTRAINT chk_vehicles_odometer
        CHECK (odometer_km >= 0)
);
```

## Valid sample records

```sql
INSERT INTO vehicles (
    registration_number,
    owner_name,
    manufacturer,
    model,
    vehicle_type,
    fuel_type,
    manufacture_year,
    purchase_date,
    color,
    odometer_km,
    insurance_expiry
)
VALUES
    (
        'TN01AB1234', 'Keerthi Rao', 'Tata', 'Nexon EV',
        'CAR', 'ELECTRIC', 2024, '2024-05-15',
        'Blue', 18400, '2027-05-14'
    ),
    (
        'KA03XY9876', 'Ravi Kumar', 'Honda', 'Activa',
        'MOTORCYCLE', 'PETROL', 2022, NULL,
        'White', 9600, NULL
    );
```

## Constraint tests

```sql
-- Expected to fail in strict mode: unsupported vehicle type.
/*
INSERT INTO vehicles (
    registration_number, owner_name, manufacturer, model,
    vehicle_type, fuel_type, manufacture_year,
    color, odometer_km
)
VALUES (
    'TEST000001', 'Invalid Owner', 'Test', 'Test Model',
    'AIRCRAFT', 'PETROL', 2026,
    'Black', 0
);
*/

-- Expected to fail: an unsigned odometer cannot be negative.
/*
INSERT INTO vehicles (
    registration_number, owner_name, manufacturer, model,
    vehicle_type, fuel_type, manufacture_year,
    color, odometer_km
)
VALUES (
    'TEST000002', 'Invalid Odometer', 'Test', 'Test Model',
    'CAR', 'PETROL', 2026,
    'Black', -1
);
*/
```

---

# Solution 8: Hotel Room Inventory

## Create `hotel_rooms`

```sql
CREATE TABLE hotel_rooms (
    room_id INT UNSIGNED AUTO_INCREMENT,
    room_number VARCHAR(10) NOT NULL,
    room_type ENUM(
        'SINGLE',
        'DOUBLE',
        'DELUXE',
        'SUITE'
    ) NOT NULL,
    floor_number SMALLINT NOT NULL,
    bed_count TINYINT UNSIGNED NOT NULL,
    max_occupancy TINYINT UNSIGNED NOT NULL,
    price_per_night DECIMAL(10, 2) NOT NULL,
    availability_status ENUM(
        'AVAILABLE',
        'RESERVED',
        'OCCUPIED',
        'MAINTENANCE'
    ) NOT NULL DEFAULT 'AVAILABLE',
    has_air_conditioning BOOLEAN NOT NULL DEFAULT TRUE,
    smoking_allowed BOOLEAN NOT NULL DEFAULT FALSE,
    notes VARCHAR(255) NULL,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL
        DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,

    CONSTRAINT pk_hotel_rooms
        PRIMARY KEY (room_id),

    CONSTRAINT uq_hotel_rooms_room_number
        UNIQUE (room_number),

    CONSTRAINT chk_hotel_rooms_bed_count
        CHECK (bed_count >= 1),

    CONSTRAINT chk_hotel_rooms_max_occupancy
        CHECK (max_occupancy >= 1),

    CONSTRAINT chk_hotel_rooms_price
        CHECK (price_per_night > 0)
);
```

## Valid sample records

```sql
INSERT INTO hotel_rooms (
    room_number,
    room_type,
    floor_number,
    bed_count,
    max_occupancy,
    price_per_night,
    availability_status,
    has_air_conditioning,
    smoking_allowed,
    notes
)
VALUES
    (
        '101', 'SINGLE', 1, 1, 1,
        2500.00, 'AVAILABLE', TRUE, FALSE,
        'Garden-facing room'
    ),
    (
        '305', 'SUITE', 3, 2, 4,
        9500.00, 'RESERVED', TRUE, FALSE,
        'Includes a separate living area'
    );
```

## Constraint tests

```sql
-- Expected to fail: occupancy must be at least one.
/*
INSERT INTO hotel_rooms (
    room_number, room_type, floor_number,
    bed_count, max_occupancy, price_per_night
)
VALUES (
    '999', 'SINGLE', 9, 1, 0, 2000.00
);
*/

-- Expected to fail: price must be greater than zero.
/*
INSERT INTO hotel_rooms (
    room_number, room_type, floor_number,
    bed_count, max_occupancy, price_per_night
)
VALUES (
    '998', 'DOUBLE', 9, 2, 2, 0.00
);
*/
```

---

# Solution 9: Movie Catalogue

## Create `movies`

```sql
CREATE TABLE movies (
    movie_id BIGINT UNSIGNED AUTO_INCREMENT,
    movie_code VARCHAR(12) NOT NULL,
    title VARCHAR(200) NOT NULL,
    genre VARCHAR(60) NOT NULL,
    original_language VARCHAR(40) NOT NULL,
    release_date DATE NULL,
    duration_minutes SMALLINT UNSIGNED NOT NULL,
    director_name VARCHAR(120) NOT NULL,
    age_certificate ENUM(
        'ALL_AGES',
        'PARENTAL_GUIDANCE',
        'ADULT',
        'UNRATED'
    ) NOT NULL DEFAULT 'UNRATED',
    audience_rating DECIMAL(3, 1) NULL,
    production_budget DECIMAL(15, 2) NULL,
    catalog_status ENUM(
        'UPCOMING',
        'RELEASED',
        'ARCHIVED'
    ) NOT NULL DEFAULT 'UPCOMING',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL
        DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,

    CONSTRAINT pk_movies
        PRIMARY KEY (movie_id),

    CONSTRAINT uq_movies_movie_code
        UNIQUE (movie_code),

    CONSTRAINT chk_movies_duration
        CHECK (duration_minutes > 0),

    CONSTRAINT chk_movies_rating
        CHECK (
            audience_rating IS NULL
            OR audience_rating BETWEEN 0.0 AND 10.0
        ),

    CONSTRAINT chk_movies_budget
        CHECK (
            production_budget IS NULL
            OR production_budget >= 0
        )
);
```

## Valid sample records

```sql
INSERT INTO movies (
    movie_code,
    title,
    genre,
    original_language,
    release_date,
    duration_minutes,
    director_name,
    age_certificate,
    audience_rating,
    production_budget,
    catalog_status
)
VALUES
    (
        'MOV000001', 'The Last Monsoon', 'Drama', 'Hindi',
        '2026-07-10', 128, 'Asha Verma',
        'PARENTAL_GUIDANCE', 8.2, 75000000.00, 'RELEASED'
    ),
    (
        'MOV000002', 'Orbit 2040', 'Science Fiction', 'English',
        NULL, 142, 'Daniel Lee',
        'UNRATED', NULL, NULL, 'UPCOMING'
    );
```

## Constraint tests

```sql
-- Expected to fail: rating exceeds 10.
/*
INSERT INTO movies (
    movie_code, title, genre, original_language,
    duration_minutes, director_name, audience_rating
)
VALUES (
    'MOV000003', 'Invalid Rating', 'Testing', 'English',
    90, 'Test Director', 10.5
);
*/

-- Expected to fail: duration is zero.
/*
INSERT INTO movies (
    movie_code, title, genre, original_language,
    duration_minutes, director_name
)
VALUES (
    'MOV000004', 'Invalid Duration', 'Testing', 'English',
    0, 'Test Director'
);
*/
```

---

# Solution 10: Customer Support Ticket

## Create `support_tickets`

```sql
CREATE TABLE support_tickets (
    ticket_id BIGINT UNSIGNED AUTO_INCREMENT,
    ticket_number VARCHAR(20) NOT NULL,
    requester_name VARCHAR(120) NOT NULL,
    requester_email VARCHAR(120) NOT NULL,
    subject VARCHAR(200) NOT NULL,
    description TEXT NOT NULL,
    category ENUM(
        'BILLING',
        'TECHNICAL',
        'ACCOUNT',
        'GENERAL'
    ) NOT NULL,
    priority ENUM(
        'LOW',
        'MEDIUM',
        'HIGH',
        'CRITICAL'
    ) NOT NULL DEFAULT 'MEDIUM',
    ticket_status ENUM(
        'OPEN',
        'IN_PROGRESS',
        'RESOLVED',
        'CLOSED'
    ) NOT NULL DEFAULT 'OPEN',
    assigned_agent VARCHAR(120) NULL,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    resolved_at TIMESTAMP NULL DEFAULT NULL,
    last_updated_at TIMESTAMP NOT NULL
        DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,

    CONSTRAINT pk_support_tickets
        PRIMARY KEY (ticket_id),

    CONSTRAINT uq_support_tickets_ticket_number
        UNIQUE (ticket_number),

    CONSTRAINT chk_support_tickets_resolution_time
        CHECK (
            resolved_at IS NULL
            OR resolved_at >= created_at
        )
);
```

## Valid sample records

```sql
INSERT INTO support_tickets (
    ticket_number,
    requester_name,
    requester_email,
    subject,
    description,
    category
)
VALUES (
    'TKT-2026-0001',
    'Kavya Sharma',
    'kavya.sharma@example.com',
    'Unable to reset password',
    'The reset link expires immediately after opening.',
    'ACCOUNT'
);

INSERT INTO support_tickets (
    ticket_number,
    requester_name,
    requester_email,
    subject,
    description,
    category,
    priority,
    ticket_status,
    assigned_agent,
    created_at,
    resolved_at
)
VALUES (
    'TKT-2026-0002',
    'Rohan Iyer',
    'rohan.iyer@example.com',
    'Invoice amount mismatch',
    'The invoice total differs from the displayed order total.',
    'BILLING',
    'HIGH',
    'RESOLVED',
    'Anita Support',
    '2026-09-20 09:00:00',
    '2026-09-20 11:30:00'
);
```

## Constraint tests

```sql
-- Expected to fail: resolution precedes creation.
/*
INSERT INTO support_tickets (
    ticket_number, requester_name, requester_email,
    subject, description, category,
    created_at, resolved_at
)
VALUES (
    'TKT-2026-0003', 'Invalid Time', 'invalid.time@example.com',
    'Invalid timestamps', 'Resolution is earlier than creation.',
    'GENERAL', '2026-09-20 12:00:00', '2026-09-20 11:00:00'
);
*/

-- Expected to fail in strict mode: unsupported priority.
/*
INSERT INTO support_tickets (
    ticket_number, requester_name, requester_email,
    subject, description, category, priority
)
VALUES (
    'TKT-2026-0004', 'Invalid Priority', 'invalid.priority@example.com',
    'Invalid priority', 'Priority is outside the enum.',
    'GENERAL', 'URGENT'
);
*/
```

---

# Verify the Completed Schema

## List all 10 tables

```sql
SHOW TABLES;
```

## Inspect column definitions

```sql
DESCRIBE students;
DESCRIBE products;
DESCRIBE customers;
DESCRIBE books;
DESCRIBE patients;
DESCRIBE bank_accounts;
DESCRIBE vehicles;
DESCRIBE hotel_rooms;
DESCRIBE movies;
DESCRIBE support_tickets;
```

## Inspect complete DDL and constraints

```sql
SHOW CREATE TABLE students;
SHOW CREATE TABLE products;
SHOW CREATE TABLE customers;
SHOW CREATE TABLE books;
SHOW CREATE TABLE patients;
SHOW CREATE TABLE bank_accounts;
SHOW CREATE TABLE vehicles;
SHOW CREATE TABLE hotel_rooms;
SHOW CREATE TABLE movies;
SHOW CREATE TABLE support_tickets;
```

# Key Design Decisions

1. Numeric surrogate keys use `AUTO_INCREMENT`.
2. Business identifiers such as SKU, ISBN, admission number, and ticket number use unique constraints.
3. Phone and account numbers use character types because they are identifiers, not quantities.
4. Financial values use `DECIMAL` rather than floating-point types.
5. Limited status values use `ENUM` for this MySQL-focused exercise.
6. `CHECK` constraints enforce row-level numeric and date rules.
7. Nullable attributes explicitly use `NULL`.
8. Creation and modification times use explicit timestamp clauses.

<!-- Mermaid rendering support for GitHub Pages/Jekyll. -->
<script type="module">
  import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs";

  document.querySelectorAll("pre > code.language-mermaid").forEach((code) => {
    const diagram = document.createElement("pre");
    diagram.className = "mermaid";
    diagram.textContent = code.textContent;
    code.parentElement.replaceWith(diagram);
  });

  mermaid.initialize({
    startOnLoad: false,
    securityLevel: "strict"
  });

  await mermaid.run({ querySelector: ".mermaid" });
</script>
10. No table contains a foreign key or relationship.

