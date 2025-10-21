## PLP-Final-Project-Bootcamp
# Veterinary Telemedicine & Outbreak Investigation Platform

# 🎯 Context

Problem Statement: Small-scale farmers in rural and remote areas face significant challenges in accessing timely veterinary care for their livestock. Traditional veterinary services are often geographically inaccessible, expensive, and slow to respond to disease outbreaks. This leads to:

· Delayed treatment causing animal mortality and economic losses
· Limited access to specialized veterinary expertise
· Slow response to disease outbreaks leading to rapid spread
· Poor record-keeping of animal health history
· Financial constraints in accessing quality veterinary care

Industry Context: The global veterinary telemedicine market is growing rapidly, with Africa showing particular potential due to mobile technology penetration. However, most existing solutions focus on companion animals rather than livestock, creating a significant gap in agricultural veterinary services.

# 🎯 Goal

Primary Objective: Develop a comprehensive web-based platform that bridges the gap between farmers and veterinary services through digital technology, enabling accessible, affordable, and timely animal healthcare while providing real-time disease surveillance capabilities.

Specific Goals:

1. Accessibility: Enable remote consultations via chat, audio, and video calls
2. Affordability: Reduce costs associated with physical veterinary visits
3. Timeliness: Provide immediate access to veterinary expertise
4. Disease Control: Create early warning system for disease outbreaks
5. Record Management: Maintain comprehensive electronic health records
6. Knowledge Sharing: Facilitate information exchange between stakeholders

# ⚠️ Constraints

Technical Constraints

· Platform: Web application using Flask + MySQL stack
· Compatibility: Must work on low-bandwidth connections in rural areas
· Devices: Accessible on basic smartphones and computers
· Security: HIPAA-like compliance for animal health data privacy
· Scalability: Support for thousands of concurrent users during outbreaks

Business Constraints

· Cost: Low operational costs to maintain affordability
· Regulatory: Compliance with veterinary practice regulations
· Adoption: User-friendly interface for farmers with varying digital literacy
· Integration: Potential for integration with existing agricultural systems

Operational Constraints

· Availability: 24/7 system availability for emergency consultations
· Data Collection: Standardized data collection for outbreak analysis
· Payment: Support for multiple payment methods including mobile money
· Language: Initially English, with potential for local language support

Legal & Ethical Constraints

· Data Privacy: Secure handling of farmer and animal health data
· Professional Standards: Maintain veterinary ethics and standards
· Liability: Clear terms for telemedicine consultations
· Compliance: Adherence to animal health regulations and reporting requirements

# 👥 Roles & Responsibilities

1. Farmers

Primary Role: Animal owners and primary users
Responsibilities:

· Register animals and maintain health profiles
· Book veterinary appointments
· Report disease outbreaks promptly
· Provide accurate clinical information
· Implement prescribed treatments
· Maintain payment for services

2. Veterinary Doctors

Primary Role: Service providers and medical experts
Responsibilities:

· Conduct remote consultations and diagnoses
· Provide e-prescriptions and treatment plans
· Monitor outbreak reports
· Maintain professional credentials
· Provide emergency response guidance
· Document case histories

3. Administrators

Primary Role: Platform managers and system overseers
Responsibilities:

· User management and verification
· System monitoring and maintenance
· Outbreak data analysis and reporting
· Payment processing oversight
· Platform content management
· Technical support coordination

4. System Components Role

Authentication System

· Secure user registration and login
· Role-based access control
· Session management
· Password security enforcement

Appointment Management

· Booking system with calendar integration
· Consultation type selection (chat/audio/video)
· Reminder notifications
· Status tracking

Outbreak Reporting Module

· Standardized data collection
· Geographic mapping of outbreaks
· Alert generation system
· Historical data analysis

E-Health Records

· Secure data storage
· Medical history tracking
· Prescription management
· Treatment outcome monitoring

Payment System

· Multiple payment method support
· Transaction security
· Invoice generation
· Financial reporting

# 🎯 Success Metrics

Short-term (3-6 months)

· 1,000+ registered farmers
· 50+ verified veterinary doctors
· 85% user satisfaction rate
· <24-hour average response time for consultations

Medium-term (6-12 months)

· 5,000+ active users
· 200+ veterinary professionals
· 90% successful consultation completion rate
· Reduced outbreak reporting time by 60%

Long-term (1-2 years)

· National-scale adoption
· Integration with government health systems
· Demonstrated reduction in livestock mortality rates
· Sustainable revenue model

# 🔄 Workflow Integration

The platform creates a seamless workflow:

1. Farmer Registration → Animal Profile Creation → Symptom Reporting
2. Appointment Booking → Consultation → E-Prescription → Payment
3. Outbreak Detection → Reporting → Investigation → Containment

This comprehensive approach ensures that every stakeholder has clear responsibilities and the system operates within defined constraints to achieve its public health and agricultural development goals.


# Project Structure
```
veterinary_telemedicine/
├── app.py
├── config.py
├── requirements.txt
├── static/
│   ├── css/
│   ├── js/
│   └── images/
├── templates/
│   ├── base.html
│   ├── auth/
│   ├── farmer/
│   ├── doctor/
│   └── admin/
└── models/
    └── database.py
```

Step 1: Setup Project Files

requirements.txt

```txt
Flask==2.3.3
Flask-MySQLdb==1.0.1
Flask-Login==0.6.3
Werkzeug==2.3.7
bcrypt==4.0.1
email-validator==2.0.0
```

config.py

```python
import os

class Config:
    SECRET_KEY = os.environ.get('SECRET_KEY') or 'your-secret-key-here'
    MYSQL_HOST = 'localhost'
    MYSQL_USER = 'root'
    MYSQL_PASSWORD = ''
    MYSQL_DB = 'veterinary_telemedicine'
    MYSQL_CURSORCLASS = 'DictCursor'

class DevelopmentConfig(Config):
    DEBUG = True

class ProductionConfig(Config):
    DEBUG = False

config = {
    'development': DevelopmentConfig,
    'production': ProductionConfig,
    'default': DevelopmentConfig
}
```

models/database.py

```python
from flask_mysqldb import MySQL
from flask_login import UserMixin
from werkzeug.security import generate_password_hash, check_password_hash
from flask_login import LoginManager

mysql = MySQL()
login_manager = LoginManager()

class User(UserMixin):
    def __init__(self, id, email, password, role, name, phone, address, created_at):
        self.id = id
        self.email = email
        self.password = password
        self.role = role
        self.name = name
        self.phone = phone
        self.address = address
        self.created_at = created_at

    def verify_password(self, password):
        return check_password_hash(self.password, password)

    @staticmethod
    def get_by_id(user_id):
        cur = mysql.connection.cursor()
        cur.execute("SELECT * FROM users WHERE id = %s", (user_id,))
        user_data = cur.fetchone()
        cur.close()
        
        if user_data:
            return User(
                id=user_data['id'],
                email=user_data['email'],
                password=user_data['password'],
                role=user_data['role'],
                name=user_data['name'],
                phone=user_data['phone'],
                address=user_data['address'],
                created_at=user_data['created_at']
            )
        return None

    @staticmethod
    def get_by_email(email):
        cur = mysql.connection.cursor()
        cur.execute("SELECT * FROM users WHERE email = %s", (email,))
        user_data = cur.fetchone()
        cur.close()
        
        if user_data:
            return User(
                id=user_data['id'],
                email=user_data['email'],
                password=user_data['password'],
                role=user_data['role'],
                name=user_data['name'],
                phone=user_data['phone'],
                address=user_data['address'],
                created_at=user_data['created_at']
            )
        return None

@login_manager.user_loader
def load_user(user_id):
    return User.get_by_id(user_id)
```

Step 2: Main Application File

app.py

```python
from flask import Flask, render_template, request, redirect, url_for, flash, jsonify, session
from flask_login import LoginManager, login_user, logout_user, login_required, current_user
from models.database import mysql, login_manager, User
from config import config
import bcrypt
from werkzeug.security import generate_password_hash, check_password_hash
import re

def create_app():
    app = Flask(__name__)
    app.config.from_object(config['development'])
    
    # Initialize extensions
    mysql.init_app(app)
    login_manager.init_app(app)
    login_manager.login_view = 'login'
    login_manager.login_message_category = 'info'
    
    return app

app = create_app()

# Email validation function
def is_valid_email(email):
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    return re.match(pattern, email) is not None

# Routes
@app.route('/')
def index():
    return render_template('index.html')

@app.route('/register', methods=['GET', 'POST'])
def register():
    if request.method == 'POST':
        name = request.form['name']
        email = request.form['email']
        phone = request.form['phone']
        address = request.form['address']
        password = request.form['password']
        confirm_password = request.form['confirm_password']
        role = request.form['role']
        
        # Validation
        if not all([name, email, phone, address, password, confirm_password]):
            flash('All fields are required!', 'danger')
            return render_template('auth/register.html')
        
        if not is_valid_email(email):
            flash('Please enter a valid email address!', 'danger')
            return render_template('auth/register.html')
        
        if password != confirm_password:
            flash('Passwords do not match!', 'danger')
            return render_template('auth/register.html')
        
        if len(password) < 6:
            flash('Password must be at least 6 characters long!', 'danger')
            return render_template('auth/register.html')
        
        # Check if user already exists
        cur = mysql.connection.cursor()
        cur.execute("SELECT id FROM users WHERE email = %s", (email,))
        existing_user = cur.fetchone()
        
        if existing_user:
            flash('Email already registered!', 'danger')
            cur.close()
            return render_template('auth/register.html')
        
        # Create new user
        hashed_password = generate_password_hash(password)
        
        cur.execute("""
            INSERT INTO users (name, email, phone, address, password, role, created_at)
            VALUES (%s, %s, %s, %s, %s, %s, NOW())
        """, (name, email, phone, address, hashed_password, role))
        
        mysql.connection.commit()
        cur.close()
        
        flash('Registration successful! Please login.', 'success')
        return redirect(url_for('login'))
    
    return render_template('auth/register.html')

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        email = request.form['email']
        password = request.form['password']
        remember = True if request.form.get('remember') else False
        
        user = User.get_by_email(email)
        
        if user and check_password_hash(user.password, password):
            login_user(user, remember=remember)
            flash('Login successful!', 'success')
            
            # Redirect based on role
            if user.role == 'farmer':
                return redirect(url_for('farmer_dashboard'))
            elif user.role == 'doctor':
                return redirect(url_for('doctor_dashboard'))
            elif user.role == 'admin':
                return redirect(url_for('admin_dashboard'))
        else:
            flash('Invalid email or password!', 'danger')
    
    return render_template('auth/login.html')

@app.route('/logout')
@login_required
def logout():
    logout_user()
    flash('You have been logged out successfully!', 'success')
    return redirect(url_for('index'))

# Dashboard Routes
@app.route('/farmer/dashboard')
@login_required
def farmer_dashboard():
    if current_user.role != 'farmer':
        flash('Access denied!', 'danger')
        return redirect(url_for('index'))
    
    cur = mysql.connection.cursor()
    
    # Get farmer's animals
    cur.execute("SELECT * FROM animals WHERE farmer_id = %s", (current_user.id,))
    animals = cur.fetchall()
    
    # Get appointments
    cur.execute("""
        SELECT a.*, u.name as doctor_name 
        FROM appointments a 
        JOIN users u ON a.doctor_id = u.id 
        WHERE a.farmer_id = %s 
        ORDER BY a.appointment_date DESC
    """, (current_user.id,))
    appointments = cur.fetchall()
    
    # Get outbreak reports
    cur.execute("SELECT * FROM outbreak_reports WHERE farmer_id = %s ORDER BY created_at DESC", (current_user.id,))
    outbreaks = cur.fetchall()
    
    cur.close()
    
    return render_template('farmer/dashboard.html', 
                         animals=animals, 
                         appointments=appointments, 
                         outbreaks=outbreaks)

@app.route('/doctor/dashboard')
@login_required
def doctor_dashboard():
    if current_user.role != 'doctor':
        flash('Access denied!', 'danger')
        return redirect(url_for('index'))
    
    cur = mysql.connection.cursor()
    
    # Get appointments
    cur.execute("""
        SELECT a.*, u.name as farmer_name, u.phone as farmer_phone
        FROM appointments a 
        JOIN users u ON a.farmer_id = u.id 
        WHERE a.doctor_id = %s 
        ORDER BY a.appointment_date DESC
    """, (current_user.id,))
    appointments = cur.fetchall()
    
    # Get e-prescriptions
    cur.execute("SELECT * FROM e_prescriptions WHERE doctor_id = %s ORDER BY created_at DESC", (current_user.id,))
    prescriptions = cur.fetchall()
    
    cur.close()
    
    return render_template('doctor/dashboard.html', 
                         appointments=appointments, 
                         prescriptions=prescriptions)

@app.route('/admin/dashboard')
@login_required
def admin_dashboard():
    if current_user.role != 'admin':
        flash('Access denied!', 'danger')
        return redirect(url_for('index'))
    
    cur = mysql.connection.cursor()
    
    # Get statistics
    cur.execute("SELECT COUNT(*) as total FROM users WHERE role = 'farmer'")
    total_farmers = cur.fetchone()['total']
    
    cur.execute("SELECT COUNT(*) as total FROM users WHERE role = 'doctor'")
    total_doctors = cur.fetchone()['total']
    
    cur.execute("SELECT COUNT(*) as total FROM appointments")
    total_appointments = cur.fetchone()['total']
    
    cur.execute("SELECT COUNT(*) as total FROM outbreak_reports")
    total_outbreaks = cur.fetchone()['total']
    
    # Get recent outbreaks
    cur.execute("""
        SELECT o.*, u.name as farmer_name 
        FROM outbreak_reports o 
        JOIN users u ON o.farmer_id = u.id 
        ORDER BY o.created_at DESC 
        LIMIT 10
    """)
    recent_outbreaks = cur.fetchall()
    
    cur.close()
    
    return render_template('admin/dashboard.html',
                         total_farmers=total_farmers,
                         total_doctors=total_doctors,
                         total_appointments=total_appointments,
                         total_outbreaks=total_outbreaks,
                         recent_outbreaks=recent_outbreaks)

# Error handlers
@app.errorhandler(404)
def not_found_error(error):
    return render_template('errors/404.html'), 404

@app.errorhandler(500)
def internal_error(error):
    return render_template('errors/500.html'), 500

if __name__ == '__main__':
    app.run(debug=True)
```

Step 3: Database Schema

Create the SQL file database_schema.sql:

```sql
-- Create database
CREATE DATABASE IF NOT EXISTS veterinary_telemedicine;
USE veterinary_telemedicine;

-- Users table
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(20) NOT NULL,
    address TEXT NOT NULL,
    password VARCHAR(255) NOT NULL,
    role ENUM('farmer', 'doctor', 'admin') NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Animals table
CREATE TABLE animals (
    id INT AUTO_INCREMENT PRIMARY KEY,
    farmer_id INT NOT NULL,
    species VARCHAR(100) NOT NULL,
    breed VARCHAR(100),
    age VARCHAR(50),
    weight DECIMAL(5,2),
    name VARCHAR(100),
    health_status ENUM('healthy', 'sick', 'recovering') DEFAULT 'healthy',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (farmer_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Appointments table
CREATE TABLE appointments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    farmer_id INT NOT NULL,
    doctor_id INT NOT NULL,
    animal_id INT,
    appointment_date DATETIME NOT NULL,
    status ENUM('pending', 'confirmed', 'completed', 'cancelled') DEFAULT 'pending',
    consultation_type ENUM('chat', 'audio', 'video') NOT NULL,
    symptoms TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (farmer_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (doctor_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (animal_id) REFERENCES animals(id) ON DELETE SET NULL
);

-- E-Prescriptions table
CREATE TABLE e_prescriptions (
    id INT AUTO_INCREMENT PRIMARY KEY,
    appointment_id INT NOT NULL,
    doctor_id INT NOT NULL,
    farmer_id INT NOT NULL,
    animal_id INT,
    diagnosis TEXT NOT NULL,
    medication TEXT NOT NULL,
    dosage TEXT NOT NULL,
    instructions TEXT,
    follow_up_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (appointment_id) REFERENCES appointments(id) ON DELETE CASCADE,
    FOREIGN KEY (doctor_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (farmer_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (animal_id) REFERENCES animals(id) ON DELETE SET NULL
);

-- Outbreak reports table
CREATE TABLE outbreak_reports (
    id INT AUTO_INCREMENT PRIMARY KEY,
    farmer_id INT NOT NULL,
    species_affected VARCHAR(100) NOT NULL,
    number_infected INT NOT NULL,
    total_animals_owned INT NOT NULL,
    previous_history TEXT,
    clinical_signs TEXT NOT NULL,
    measures_taken TEXT,
    local_disease_name VARCHAR(100),
    status ENUM('reported', 'investigating', 'contained', 'resolved') DEFAULT 'reported',
    location VARCHAR(255),
    latitude DECIMAL(10,8),
    longitude DECIMAL(11,8),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (farmer_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Payments table
CREATE TABLE payments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    appointment_id INT NOT NULL,
    amount DECIMAL(10,2) NOT NULL,
    payment_method ENUM('mpesa', 'card', 'bank_transfer') NOT NULL,
    transaction_id VARCHAR(100),
    status ENUM('pending', 'completed', 'failed') DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (appointment_id) REFERENCES appointments(id) ON DELETE CASCADE
);

-- Chat messages table
CREATE TABLE chat_messages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    appointment_id INT NOT NULL,
    sender_id INT NOT NULL,
    message TEXT NOT NULL,
    message_type ENUM('text', 'image', 'file') DEFAULT 'text',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (appointment_id) REFERENCES appointments(id) ON DELETE CASCADE,
    FOREIGN KEY (sender_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Create indexes for better performance
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_role ON users(role);
CREATE INDEX idx_appointments_farmer ON appointments(farmer_id);
CREATE INDEX idx_appointments_doctor ON appointments(doctor_id);
CREATE INDEX idx_appointments_date ON appointments(appointment_date);
CREATE INDEX idx_outbreaks_farmer ON outbreak_reports(farmer_id);
CREATE INDEX idx_outbreaks_status ON outbreak_reports(status);

-- Insert sample data
INSERT INTO users (name, email, phone, address, password, role) VALUES
('Admin User', 'admin@vettelemed.com', '+254700000000', 'Nairobi, Kenya', '$2b$12$LQv3c1yqBWVHxkd0LHAkCOYz6TtxMQJqhN8/LewdBPj89tiM1FEyG', 'admin'),
('Dr. John Kimani', 'dr.kimani@vettelemed.com', '+254711111111', 'Kisumu, Kenya', '$2b$12$LQv3c1yqBWVHxkd0LHAkCOYz6TtxMQJqhN8/LewdBPj89tiM1FEyG', 'doctor'),
('Dr. Mary Wambui', 'dr.wambui@vettelemed.com', '+254722222222', 'Nakuru, Kenya', '$2b$12$LQv3c1yqBWVHxkd0LHAkCOYz6TtxMQJqhN8/LewdBPj89tiM1FEyG', 'doctor'),
('Farmer James Omondi', 'james@farm.com', '+254733333333', 'Eldoret, Kenya', '$2b$12$LQv3c1yqBWVHxkd0LHAkCOYz6TtxMQJqhN8/LewdBPj89tiM1FEyG', 'farmer');
```

Step 4: Templates

templates/base.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Veterinary Telemedicine{% endblock %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="{{ url_for('static', filename='css/style.css') }}" rel="stylesheet">
</head>
<body>
    <!-- Navigation -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
        <div class="container">
            <a class="navbar-brand" href="{{ url_for('index') }}">
                <i class="fas fa-stethoscope me-2"></i>
                VetTeleMed
            </a>
            
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
                <span class="navbar-toggler-icon"></span>
            </button>
            
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav me-auto">
                    <li class="nav-item">
                        <a class="nav-link" href="{{ url_for('index') }}">Home</a>
                    </li>
                    {% if current_user.is_authenticated %}
                        {% if current_user.role == 'farmer' %}
                            <li class="nav-item">
                                <a class="nav-link" href="{{ url_for('farmer_dashboard') }}">Dashboard</a>
                            </li>
                        {% elif current_user.role == 'doctor' %}
                            <li class="nav-item">
                                <a class="nav-link" href="{{ url_for('doctor_dashboard') }}">Dashboard</a>
                            </li>
                        {% elif current_user.role == 'admin' %}
                            <li class="nav-item">
                                <a class="nav-link" href="{{ url_for('admin_dashboard') }}">Dashboard</a>
                            </li>
                        {% endif %}
                    {% endif %}
                </ul>
                
                <ul class="navbar-nav">
                    {% if current_user.is_authenticated %}
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                                <i class="fas fa-user me-1"></i> {{ current_user.name }}
                            </a>
                            <ul class="dropdown-menu">
                                <li><a class="dropdown-item" href="#"><i class="fas fa-cog me-2"></i>Settings</a></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="{{ url_for('logout') }}"><i class="fas fa-sign-out-alt me-2"></i>Logout</a></li>
                            </ul>
                        </li>
                    {% else %}
                        <li class="nav-item">
                            <a class="nav-link" href="{{ url_for('login') }}">Login</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="{{ url_for('register') }}">Register</a>
                        </li>
                    {% endif %}
                </ul>
            </div>
        </div>
    </nav>

    <!-- Flash Messages -->
    <div class="container mt-3">
        {% with messages = get_flashed_messages(with_categories=true) %}
            {% if messages %}
                {% for category, message in messages %}
                    <div class="alert alert-{{ 'danger' if category == 'error' else category }} alert-dismissible fade show">
                        {{ message }}
                        <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
                    </div>
                {% endfor %}
            {% endif %}
        {% endwith %}
    </div>

    <!-- Main Content -->
    <main>
        {% block content %}{% endblock %}
    </main>

    <!-- Footer -->
    <footer class="bg-dark text-light mt-5 py-4">
        <div class="container text-center">
            <p>&copy; 2024 Veterinary Telemedicine Platform. All rights reserved.</p>
        </div>
    </footer>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    <script src="{{ url_for('static', filename='js/script.js') }}"></script>
    {% block scripts %}{% endblock %}
</body>
</html>
```

templates/auth/register.html

```html
{% extends "base.html" %}

{% block title %}Register - VetTeleMed{% endblock %}

{% block content %}
<div class="container mt-5">
    <div class="row justify-content-center">
        <div class="col-md-8">
            <div class="card shadow">
                <div class="card-header bg-primary text-white">
                    <h4 class="mb-0"><i class="fas fa-user-plus me-2"></i>Create Account</h4>
                </div>
                <div class="card-body">
                    <form method="POST" action="{{ url_for('register') }}">
                        <div class="row">
                            <div class="col-md-6 mb-3">
                                <label for="name" class="form-label">Full Name</label>
                                <input type="text" class="form-control" id="name" name="name" required>
                            </div>
                            <div class="col-md-6 mb-3">
                                <label for="email" class="form-label">Email Address</label>
                                <input type="email" class="form-control" id="email" name="email" required>
                            </div>
                        </div>
                        
                        <div class="row">
                            <div class="col-md-6 mb-3">
                                <label for="phone" class="form-label">Phone Number</label>
                                <input type="tel" class="form-control" id="phone" name="phone" required>
                            </div>
                            <div class="col-md-6 mb-3">
                                <label for="role" class="form-label">I am a</label>
                                <select class="form-select" id="role" name="role" required>
                                    <option value="">Select Role</option>
                                    <option value="farmer">Farmer</option>
                                    <option value="doctor">Veterinary Doctor</option>
                                </select>
                            </div>
                        </div>
                        
                        <div class="mb-3">
                            <label for="address" class="form-label">Physical Address</label>
                            <textarea class="form-control" id="address" name="address" rows="3" required></textarea>
                        </div>
                        
                        <div class="row">
                            <div class="col-md-6 mb-3">
                                <label for="password" class="form-label">Password</label>
                                <input type="password" class="form-control" id="password" name="password" required>
                                <div class="form-text">Must be at least 6 characters long.</div>
                            </div>
                            <div class="col-md-6 mb-3">
                                <label for="confirm_password" class="form-label">Confirm Password</label>
                                <input type="password" class="form-control" id="confirm_password" name="confirm_password" required>
                            </div>
                        </div>
                        
                        <div class="mb-3 form-check">
                            <input type="checkbox" class="form-check-input" id="terms" required>
                            <label class="form-check-label" for="terms">
                                I agree to the <a href="#">Terms of Service</a> and <a href="#">Privacy Policy</a>
                            </label>
                        </div>
                        
                        <button type="submit" class="btn btn-primary btn-lg w-100">
                            <i class="fas fa-user-plus me-2"></i>Create Account
                        </button>
                    </form>
                    
                    <div class="text-center mt-3">
                        <p>Already have an account? <a href="{{ url_for('login') }}">Login here</a></p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

templates/auth/login.html

```html
{% extends "base.html" %}

{% block title %}Login - VetTeleMed{% endblock %}

{% block content %}
<div class="container mt-5">
    <div class="row justify-content-center">
        <div class="col-md-6">
            <div class="card shadow">
                <div class="card-header bg-primary text-white">
                    <h4 class="mb-0"><i class="fas fa-sign-in-alt me-2"></i>Login to Your Account</h4>
                </div>
                <div class="card-body">
                    <form method="POST" action="{{ url_for('login') }}">
                        <div class="mb-3">
                            <label for="email" class="form-label">Email Address</label>
                            <input type="email" class="form-control" id="email" name="email" required>
                        </div>
                        
                        <div class="mb-3">
                            <label for="password" class="form-label">Password</label>
                            <input type="password" class="form-control" id="password" name="password" required>
                        </div>
                        
                        <div class="mb-3 form-check">
                            <input type="checkbox" class="form-check-input" id="remember" name="remember">
                            <label class="form-check-label" for="remember">Remember me</label>
                        </div>
                        
                        <button type="submit" class="btn btn-primary btn-lg w-100">
                            <i class="fas fa-sign-in-alt me-2"></i>Login
                        </button>
                    </form>
                    
                    <div class="text-center mt-3">
                        <p>Don't have an account? <a href="{{ url_for('register') }}">Register here</a></p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

Step 5: Static Files

static/css/style.css

```css
:root {
    --primary-color: #2c3e50;
    --secondary-color: #3498db;
    --success-color: #27ae60;
    --warning-color: #f39c12;
    --danger-color: #e74c3c;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #f8f9fa;
}

.card {
    border: none;
    border-radius: 15px;
    transition: transform 0.3s ease;
}

.card:hover {
    transform: translateY(-5px);
}

.card-header {
    border-radius: 15px 15px 0 0 !important;
}

.btn-primary {
    background-color: var(--secondary-color);
    border-color: var(--secondary-color);
}

.btn-primary:hover {
    background-color: #2980b9;
    border-color: #2980b9;
}

.navbar-brand {
    font-weight: bold;
    font-size: 1.5rem;
}

.stat-card {
    text-align: center;
    padding: 20px;
}

.stat-card i {
    font-size: 2.5rem;
    margin-bottom: 10px;
}

.stat-card .number {
    font-size: 2rem;
    font-weight: bold;
    display: block;
}

.alert {
    border-radius: 10px;
    border: none;
}

.table th {
    border-top: none;
    font-weight: 600;
}

.form-control, .form-select {
    border-radius: 8px;
    padding: 10px 15px;
}

.form-control:focus, .form-select:focus {
    box-shadow: 0 0 0 0.2rem rgba(52, 152, 219, 0.25);
    border-color: var(--secondary-color);
}

/* Dashboard specific styles */
.dashboard-header {
    background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
    color: white;
    padding: 30px 0;
    margin-bottom: 30px;
    border-radius: 0 0 20px 20px;
}

.quick-actions .btn {
    margin: 5px;
    border-radius: 10px;
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .card {
        margin-bottom: 20px;
    }
    
    .dashboard-header h1 {
        font-size: 1.8rem;
    }
}
```

static/js/script.js

```javascript
// General JavaScript utilities
document.addEventListener('DOMContentLoaded', function() {
    // Auto-dismiss alerts after 5 seconds
    const alerts = document.querySelectorAll('.alert');
    alerts.forEach(alert => {
        setTimeout(() => {
            const bsAlert = new bootstrap.Alert(alert);
            bsAlert.close();
        }, 5000);
    });

    // Password confirmation validation
    const password = document.getElementById('password');
    const confirmPassword = document.getElementById('confirm_password');
    
    if (password && confirmPassword) {
        function validatePassword() {
            if (password.value !== confirmPassword.value) {
                confirmPassword.setCustomValidity("Passwords don't match");
            } else {
                confirmPassword.setCustomValidity('');
            }
        }
        
        password.addEventListener('change', validatePassword);
        confirmPassword.addEventListener('keyup', validatePassword);
    }

    // Form validation enhancements
    const forms = document.querySelectorAll('form');
    forms.forEach(form => {
        form.addEventListener('submit', function(e) {
            if (!form.checkValidity()) {
                e.preventDefault();
                e.stopPropagation();
            }
            form.classList.add('was-validated');
        });
    });
});

// Utility functions
function formatDate(dateString) {
    const options = { 
        year: 'numeric', 
        month: 'short', 
        day: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
    };
    return new Date(dateString).toLocaleDateString('en-US', options);
}

function showLoading() {
    // Implement loading indicator
    const loader = document.createElement('div');
    loader.className = 'loading-overlay';
    loader.innerHTML = `
        <div class="spinner-border text-primary" role="status">
            <span class="visually-hidden">Loading...</span>
        </div>
    `;
    document.body.appendChild(loader);
}

function hideLoading() {
    const loader = document.querySelector('.loading-overlay');
    if (loader) {
        loader.remove();
    }
}
```

Step 6: Setup Instructions

1. Install Dependencies:

```bash
pip install -r requirements.txt
```

1. Database Setup:
   · Install MySQL and create database
   · Run the database_schema.sql file
   · Update database credentials in config.py
2. Run the Application:

```bash
python app.py
```

Key Features Implemented:

✅ User Authentication System

· Registration with role selection (Farmer/Doctor/Admin)
· Login with session management
· Password hashing and validation
· Role-based access control

✅ Database Schema

· Comprehensive tables for all features
· Proper relationships and constraints
· Sample data for testing

✅ Dashboard Structure

· Role-specific dashboards
· CRUD operations ready framework
· Responsive design

✅ Security Features

· Password hashing with Werkzeug
· Session management with Flask-Login
· Input validation and sanitization
· Role-based access control
