<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Comprehensive User Registration</title>
  <!-- Google Fonts & FontAwesome -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

  <style>
    :root {
      --primary-color: #4f46e5;
      --primary-hover: #4338ca;
      --bg-color: #f3f4f6;
      --card-bg: #ffffff;
      --text-main: #1f2937;
      --text-muted: #6b7280;
      --border-color: #e5e7eb;
      --input-bg: #f9fafb;
      --focus-ring: rgba(79, 70, 229, 0.2);
      --error-color: #ef4444;
      --success-color: #10b981;
      --warning-color: #f59e0b;
      --shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
      --radius: 12px;
      --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    }

    [data-theme="dark"] {
      --bg-color: #0f172a;
      --card-bg: #1e293b;
      --text-main: #f8fafc;
      --text-muted: #94a3b8;
      --border-color: #334155;
      --input-bg: #0f172a;
      --focus-ring: rgba(99, 102, 241, 0.3);
      --shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.5);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Inter', sans-serif;
    }

    body {
      background-color: var(--bg-color);
      color: var(--text-main);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 2rem 1rem;
      transition: background-color 0.3s ease, color 0.3s ease;
    }

    .container {
      width: 100%;
      max-width: 800px;
      background-color: var(--card-bg);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 2.5rem;
      position: relative;
      overflow: hidden;
      transition: var(--transition);
    }

    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 2rem;
    }

    .header-text h1 {
      font-size: 1.75rem;
      font-weight: 700;
      margin-bottom: 0.25rem;
    }

    .header-text p {
      color: var(--text-muted);
      font-size: 0.9rem;
    }

    .theme-toggle {
      background: transparent;
      border: 1px solid var(--border-color);
      color: var(--text-main);
      padding: 0.5rem 0.75rem;
      border-radius: 8px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 0.5rem;
      font-size: 0.85rem;
      transition: var(--transition);
    }

    .theme-toggle:hover {
      border-color: var(--primary-color);
      color: var(--primary-color);
    }

    /* Progress Tracker */
    .progressbar {
      display: flex;
      justify-content: space-between;
      margin-bottom: 2.5rem;
      position: relative;
    }

    .progressbar::before {
      content: "";
      position: absolute;
      top: 50%;
      left: 0;
      transform: translateY(-50%);
      height: 4px;
      width: 100%;
      background-color: var(--border-color);
      z-index: 1;
    }

    .progress-line {
      position: absolute;
      top: 50%;
      left: 0;
      transform: translateY(-50%);
      height: 4px;
      background-color: var(--primary-color);
      z-index: 1;
      width: 0%;
      transition: width 0.4s ease;
    }

    .progress-step {
      width: 35px;
      height: 35px;
      background-color: var(--card-bg);
      border: 3px solid var(--border-color);
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      font-weight: 600;
      font-size: 0.85rem;
      z-index: 2;
      transition: var(--transition);
      color: var(--text-muted);
      position: relative;
    }

    .progress-step.active {
      border-color: var(--primary-color);
      background-color: var(--primary-color);
      color: white;
    }

    .progress-step.completed {
      border-color: var(--primary-color);
      background-color: var(--primary-color);
      color: white;
    }

    .step-label {
      position: absolute;
      top: 42px;
      font-size: 0.75rem;
      font-weight: 500;
      white-space: nowrap;
      color: var(--text-muted);
    }

    .progress-step.active .step-label {
      color: var(--primary-color);
      font-weight: 600;
    }

    /* Step content handling */
    .form-step {
      display: none;
      animation: fadeIn 0.4s ease-in-out;
    }

    .form-step.active {
      display: block;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

    /* Grid Layouts */
    .grid-2 {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 1.25rem;
    }

    .grid-3 {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 1.25rem;
    }

    @media (max-width: 640px) {
      .grid-2, .grid-3 {
        grid-template-columns: 1fr;
      }
      .container { padding: 1.5rem; }
      .step-label { display: none; }
    }

    /* Form Elements */
    .form-group {
      margin-bottom: 1.25rem;
      display: flex;
      flex-direction: column;
    }

    .form-group.full-width {
      grid-column: 1 / -1;
    }

    label {
      font-size: 0.875rem;
      font-weight: 500;
      margin-bottom: 0.5rem;
      color: var(--text-main);
      display: flex;
      justify-content: space-between;
    }

    .required {
      color: var(--error-color);
    }

    .input-wrapper {
      position: relative;
      display: flex;
      align-items: center;
    }

    .input-wrapper i {
      position: absolute;
      left: 1rem;
      color: var(--text-muted);
      font-size: 0.9rem;
    }

    input[type="text"],
    input[type="email"],
    input[type="password"],
    input[type="tel"],
    input[type="date"],
    input[type="url"],
    select,
    textarea {
      width: 100%;
      padding: 0.75rem 1rem;
      padding-left: 2.5rem;
      border: 1px solid var(--border-color);
      border-radius: 8px;
      background-color: var(--input-bg);
      color: var(--text-main);
      font-size: 0.9rem;
      outline: none;
      transition: var(--transition);
    }

    .no-icon input,
    .no-icon select,
    .no-icon textarea {
      padding-left: 1rem;
    }

    input:focus, select:focus, textarea:focus {
      border-color: var(--primary-color);
      box-shadow: 0 0 0 3px var(--focus-ring);
    }

    .toggle-password {
      position: absolute;
      right: 1rem;
      left: auto !important;
      cursor: pointer;
    }

    /* Range slider */
    .range-group {
      display: flex;
      align-items: center;
      gap: 1rem;
    }

    input[type="range"] {
      flex: 1;
      accent-color: var(--primary-color);
    }

    .range-val {
      font-weight: 600;
      min-width: 40px;
    }

    /* Password Strength */
    .strength-meter {
      height: 4px;
      background-color: var(--border-color);
      margin-top: 0.5rem;
      border-radius: 2px;
      overflow: hidden;
    }

    .strength-bar {
      height: 100%;
      width: 0;
      transition: var(--transition);
    }

    .strength-text {
      font-size: 0.75rem;
      color: var(--text-muted);
      margin-top: 0.25rem;
    }

    /* Radio & Checkbox Card Groups */
    .options-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
      gap: 0.75rem;
    }

    .option-card {
      position: relative;
    }

    .option-card input {
      position: absolute;
      opacity: 0;
      width: 0;
      height: 0;
    }

    .card-label {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 1rem;
      border: 1px solid var(--border-color);
      border-radius: 8px;
      background-color: var(--input-bg);
      cursor: pointer;
      transition: var(--transition);
      text-align: center;
      gap: 0.5rem;
      height: 100%;
    }

    .card-label i {
      font-size: 1.25rem;
      color: var(--text-muted);
    }

    .option-card input:checked + .card-label {
      border-color: var(--primary-color);
      background-color: rgba(79, 70, 229, 0.05);
      color: var(--primary-color);
    }

    .option-card input:checked + .card-label i {
      color: var(--primary-color);
    }

    /* File Upload */
    .file-upload-box {
      border: 2px dashed var(--border-color);
      padding: 1.5rem;
      border-radius: 8px;
      text-align: center;
      background-color: var(--input-bg);
      cursor: pointer;
      transition: var(--transition);
    }

    .file-upload-box:hover {
      border-color: var(--primary-color);
    }

    .file-upload-box i {
      font-size: 2rem;
      color: var(--text-muted);
      margin-bottom: 0.5rem;
    }

    .file-upload-box p {
      font-size: 0.85rem;
      color: var(--text-muted);
    }

    input[type="file"] {
      display: none;
    }

    /* Switch Component */
    .switch-container {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0.75rem 0;
    }

    .switch {
      position: relative;
      display: inline-block;
      width: 44px;
      height: 24px;
    }

    .switch input {
      opacity: 0;
      width: 0;
      height: 0;
    }

    .slider {
      position: absolute;
      cursor: pointer;
      top: 0; left: 0; right: 0; bottom: 0;
      background-color: var(--border-color);
      transition: .4s;
      border-radius: 24px;
    }

    .slider:before {
      position: absolute;
      content: "";
      height: 18px;
      width: 18px;
      left: 3px;
      bottom: 3px;
      background-color: white;
      transition: .4s;
      border-radius: 50%;
    }

    input:checked + .slider {
      background-color: var(--primary-color);
    }

    input:checked + .slider:before {
      transform: translateX(20px);
    }

    /* Error Message Styling */
    .error-msg {
      color: var(--error-color);
      font-size: 0.75rem;
      margin-top: 0.25rem;
      display: none;
    }

    .has-error input, .has-error select, .has-error textarea {
      border-color: var(--error-color);
    }

    .has-error .error-msg {
      display: block;
    }

    /* Navigation Buttons */
    .btn-group {
      display: flex;
      justify-content: space-between;
      margin-top: 2rem;
      gap: 1rem;
    }

    .btn {
      padding: 0.75rem 1.5rem;
      border-radius: 8px;
      font-weight: 600;
      font-size: 0.9rem;
      cursor: pointer;
      border: none;
      transition: var(--transition);
      display: flex;
      align-items: center;
      gap: 0.5rem;
    }

    .btn-primary {
      background-color: var(--primary-color);
      color: white;
      margin-left: auto;
    }

    .btn-primary:hover {
      background-color: var(--primary-hover);
    }

    .btn-secondary {
      background-color: transparent;
      border: 1px solid var(--border-color);
      color: var(--text-main);
    }

    .btn-secondary:hover {
      background-color: var(--border-color);
    }

    /* Success Screen */
    .success-screen {
      text-align: center;
      padding: 2rem 0;
      display: none;
    }

    .success-icon {
      width: 70px;
      height: 70px;
      background-color: rgba(16, 185, 129, 0.1);
      color: var(--success-color);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 2rem;
      margin: 0 auto 1.5rem;
    }

    .summary-box {
      margin-top: 1.5rem;
      background-color: var(--input-bg);
      border: 1px solid var(--border-color);
      border-radius: 8px;
      padding: 1rem;
      text-align: left;
      font-size: 0.85rem;
    }

    .summary-row {
      display: flex;
      justify-content: space-between;
      padding: 0.5rem 0;
      border-bottom: 1px dashed var(--border-color);
    }

    .summary-row:last-child {
      border-bottom: none;
    }
  </style>
</head>
<body>

  <div class="container">
    <div class="header">
      <div class="header-text">
        <h1 id="form-title">Create Account</h1>
        <p id="form-subtitle">Fill in your information to register with us.</p>
      </div>
      <button class="theme-toggle" id="themeToggle" type="button">
        <i class="fa-solid fa-moon"></i>
        <span>Dark</span>
      </button>
    </div>

    <!-- Multi-Step Progress Tracker -->
    <div class="progressbar">
      <div class="progress-line" id="progressLine"></div>
      <div class="progress-step active" data-step="1">
        1
        <span class="step-label">Account</span>
      </div>
      <div class="progress-step" data-step="2">
        2
        <span class="step-label">Personal</span>
      </div>
      <div class="progress-step" data-step="3">
        3
        <span class="step-label">Preferences</span>
      </div>
      <div class="progress-step" data-step="4">
        4
        <span class="step-label">Review</span>
      </div>
    </div>

    <form id="registrationForm" novalidate>
      <!-- STEP 1: Account Info -->
      <div class="form-step active" data-step="1">
        <div class="grid-2">
          <div class="form-group">
            <label for="username">Username <span class="required">*</span></label>
            <div class="input-wrapper">
              <i class="fa-solid fa-user"></i>
              <input type="text" id="username" name="username" placeholder="johndoe" required>
            </div>
            <span class="error-msg">Username must be at least 3 characters long.</span>
          </div>

          <div class="form-group">
            <label for="email">Email Address <span class="required">*</span></label>
            <div class="input-wrapper">
              <i class="fa-solid fa-envelope"></i>
              <input type="email" id="email" name="email" placeholder="john@example.com" required>
            </div>
            <span class="error-msg">Please enter a valid email address.</span>
          </div>

          <div class="form-group">
            <label for="password">Password <span class="required">*</span></label>
            <div class="input-wrapper">
              <i class="fa-solid fa-lock"></i>
              <input type="password" id="password" name="password" placeholder="••••••••" required>
              <i class="fa-solid fa-eye toggle-password" id="togglePassword"></i>
            </div>
            <div class="strength-meter">
              <div class="strength-bar" id="strengthBar"></div>
            </div>
            <div class="strength-text" id="strengthText">Enter password strength</div>
            <span class="error-msg">Must be at least 8 chars with 1 number and 1 special char.</span>
          </div>

          <div class="form-group">
            <label for="confirmPassword">Confirm Password <span class="required">*</span></label>
            <div class="input-wrapper">
              <i class="fa-solid fa-shield-halved"></i>
              <input type="password" id="confirmPassword" name="confirmPassword" placeholder="••••••••" required>
            </div>
            <span class="error-msg">Passwords do not match.</span>
          </div>
        </div>

        <div class="form-group">
          <label>Account Type <span class="required">*</span></label>
          <div class="options-grid">
            <div class="option-card">
              <input type="radio" name="accountType" id="typePersonal" value="Personal" checked>
              <label for="typePersonal" class="card-label">
                <i class="fa-solid fa-user"></i>
                <span>Personal</span>
              </label>
            </div>
            <div class="option-card">
              <input type="radio" name="accountType" id="typeBusiness" value="Business">
              <label for="typeBusiness" class="card-label">
                <i class="fa-solid fa-briefcase"></i>
                <span>Business</span>
              </label>
            </div>
            <div class="option-card">
              <input type="radio" name="accountType" id="typeStudent" value="Student">
              <label for="typeStudent" class="card-label">
                <i class="fa-solid fa-graduation-cap"></i>
                <span>Student</span>
              </label>
            </div>
          </div>
        </div>
      </div>

      <!-- STEP 2: Personal Info & Details -->
      <div class="form-step" data-step="2">
        <div class="grid-2">
          <div class="form-group">
            <label for="firstName">First Name <span class="required">*</span></label>
            <div class="input-wrapper">
              <i class="fa-solid fa-id-card"></i>
              <input type="text" id="firstName" name="firstName" placeholder="John" required>
            </div>
            <span class="error-msg">First name is required.</span>
          </div>

          <div class="form-group">
            <label for="lastName">Last Name <span class="required">*</span></label>
            <div class="input-wrapper">
              <i class="fa-solid fa-id-card"></i>
              <input type="text" id="lastName" name="lastName" placeholder="Doe" required>
            </div>
            <span class="error-msg">Last name is required.</span>
          </div>
        </div>

        <div class="grid-2">
          <div class="form-group">
            <label for="dob">Date of Birth <span class="required">*</span></label>
            <div class="input-wrapper">
              <i class="fa-solid fa-calendar"></i>
              <input type="date" id="dob" name="dob" required>
            </div>
            <span class="error-msg">You must be at least 18 years old.</span>
          </div>

          <div class="form-group">
            <label for="phone">Phone Number</label>
            <div class="input-wrapper">
              <i class="fa-solid fa-phone"></i>
              <input type="tel" id="phone" name="phone" placeholder="+1 (555) 000-0000">
            </div>
          </div>
        </div>

        <div class="grid-2">
          <div class="form-group">
            <label for="gender">Gender Identity</label>
            <div class="input-wrapper">
              <i class="fa-solid fa-venus-mars"></i>
              <select id="gender" name="gender">
                <option value="">Select Option</option>
                <option value="Male">Male</option>
                <option value="Female">Female</option>
                <option value="Non-binary">Non-binary</option>
                <option value="Prefer not to say">Prefer not to say</option>
              </select>
            </div>
          </div>

          <div class="form-group">
            <label for="country">Country <span class="required">*</span></label>
            <div class="input-wrapper">
              <i class="fa-solid fa-globe"></i>
              <select id="country" name="country" required>
                <option value="">Select Country</option>
                <option value="United States">United States</option>
                <option value="United Kingdom">United Kingdom</option>
                <option value="Canada">Canada</option>
                <option value="Australia">Australia</option>
                <option value="India">India</option>
                <option value="Germany">Germany</option>
              </select>
            </div>
            <span class="error-msg">Please select a country.</span>
          </div>
        </div>

        <div class="form-group">
          <label for="profilePic">Profile Picture</label>
          <div class="file-upload-box" id="dropZone">
            <i class="fa-solid fa-cloud-arrow-up"></i>
            <p id="fileNameDisplay">Click or Drag & Drop image here (PNG, JPG up to 5MB)</p>
            <input type="file" id="profilePic" name="profilePic" accept="image/*">
          </div>
        </div>
      </div>

      <!-- STEP 3: Preferences & Roles -->
      <div class="form-step" data-step="3">
        <div class="form-group">
          <label>Interests / Topics of Interest</label>
          <div class="options-grid">
            <div class="option-card">
              <input type="checkbox" name="interests" id="tech" value="Technology">
              <label for="tech" class="card-label">
                <i class="fa-solid fa-laptop-code"></i>
                <span>Technology</span>
              </label>
            </div>
            <div class="option-card">
              <input type="checkbox" name="interests" id="design" value="Design">
              <label for="design" class="card-label">
                <i class="fa-solid fa-palette"></i>
                <span>Design</span>
              </label>
            </div>
            <div class="option-card">
              <input type="checkbox" name="interests" id="business" value="Business">
              <label for="business" class="card-label">
                <i class="fa-solid fa-chart-line"></i>
                <span>Business</span>
              </label>
            </div>
            <div class="option-card">
              <input type="checkbox" name="interests" id="gaming" value="Gaming">
              <label for="gaming" class="card-label">
                <i class="fa-solid fa-gamepad"></i>
                <span>Gaming</span>
              </label>
            </div>
          </div>
        </div>

        <div class="form-group">
          <label for="expLevel">Experience Level: <span id="expVal" class="range-val"> Intermediate (5 yrs)</span></label>
          <div class="range-group no-icon">
            <input type="range" id="expLevel" name="expLevel" min="0" max="10" value="5">
          </div>
        </div>

        <div class="form-group no-icon">
          <label for="bio">Short Bio</label>
          <textarea id="bio" name="bio" rows="3" placeholder="Tell us a little bit about yourself..."></textarea>
        </div>

        <div class="form-group no-icon">
          <div class="switch-container">
            <div>
              <strong style="font-size:0.9rem;">Email Notifications</strong>
              <p style="font-size:0.75rem; color:var(--text-muted);">Receive product updates and newsletters</p>
            </div>
            <label class="switch">
              <input type="checkbox" id="newsletter" name="newsletter" checked>
              <span class="slider"></span>
            </label>
          </div>

          <div class="switch-container">
            <div>
              <strong style="font-size:0.9rem;">Two-Factor Authentication (2FA)</strong>
              <p style="font-size:0.75rem; color:var(--text-muted);">Enhance your account security</p>
            </div>
            <label class="switch">
              <input type="checkbox" id="twoFactor" name="twoFactor">
              <span class="slider"></span>
            </label>
          </div>
        </div>
      </div>

      <!-- STEP 4: Terms & Review -->
      <div class="form-step" data-step="4">
        <p style="font-size: 0.9rem; color: var(--text-muted); margin-bottom: 1rem;">
          Please review your details before submitting the form.
        </p>

        <div class="summary-box" id="summaryContent">
          <!-- Dynamic summary populated via JS -->
        </div>

        <div class="form-group no-icon" style="margin-top: 1.5rem;">
          <label style="display:flex; align-items: flex-start; gap:0.5rem; font-weight: normal; cursor:pointer;">
            <input type="checkbox" id="terms" name="terms" required style="margin-top: 3px;">
            <span>I agree to the <a href="#" style="color:var(--primary-color);">Terms of Service</a> and <a href="#" style="color:var(--primary-color);">Privacy Policy</a>. <span class="required">*</span></span>
          </label>
          <span class="error-msg" id="termsError">You must accept the terms to proceed.</span>
        </div>
      </div>

      <!-- Form Actions Button Bar -->
      <div class="btn-group">
        <button type="button" class="btn btn-secondary" id="prevBtn" style="display: none;">
          <i class="fa-solid fa-arrow-left"></i> Back
        </button>
        <button type="button" class="btn btn-primary" id="nextBtn">
          Next <i class="fa-solid fa-arrow-right"></i>
        </button>
        <button type="submit" class="btn btn-primary" id="submitBtn" style="display: none;">
          Complete Registration <i class="fa-solid fa-check"></i>
        </button>
      </div>
    </form>

    <!-- Success Screen Display -->
    <div class="success-screen" id="successScreen">
      <div class="success-icon">
        <i class="fa-solid fa-check"></i>
      </div>
      <h2>Registration Successful!</h2>
      <p style="color: var(--text-muted); margin-top: 0.5rem;">Welcome aboard! We have sent a confirmation email to verify your account.</p>
      <button class="btn btn-primary" style="margin: 2rem auto 0;" onclick="location.reload()">Back to Home</button>
    </div>
  </div>

  <script>
    document.addEventListener("DOMContentLoaded", () => {
      let currentStep = 1;
      const totalSteps = 4;

      // Elements
      const form = document.getElementById("registrationForm");
      const steps = document.querySelectorAll(".form-step");
      const progressSteps = document.querySelectorAll(".progress-step");
      const progressLine = document.getElementById("progressLine");
      const nextBtn = document.getElementById("nextBtn");
      const prevBtn = document.getElementById("prevBtn");
      const submitBtn = document.getElementById("submitBtn");
      const themeToggle = document.getElementById("themeToggle");
      const passwordInput = document.getElementById("password");
      const togglePassword = document.getElementById("togglePassword");
      const expSlider = document.getElementById("expLevel");
      const expVal = document.getElementById("expVal");
      const fileInput = document.getElementById("profilePic");
      const dropZone = document.getElementById("dropZone");
      const fileNameDisplay = document.getElementById("fileNameDisplay");

      // 1. Dark/Light Theme Switching
      themeToggle.addEventListener("click", () => {
        const isDark = document.body.getAttribute("data-theme") === "dark";
        if (isDark) {
          document.body.removeAttribute("data-theme");
          themeToggle.innerHTML = `<i class="fa-solid fa-moon"></i> <span>Dark</span>`;
        } else {
          document.body.setAttribute("data-theme", "dark");
          themeToggle.innerHTML = `<i class="fa-solid fa-sun"></i> <span>Light</span>`;
        }
      });

      // 2. Password Visibility Toggle
      togglePassword.addEventListener("click", () => {
        const type = passwordInput.getAttribute("type") === "password" ? "text" : "password";
        passwordInput.setAttribute("type", type);
        togglePassword.classList.toggle("fa-eye");
        togglePassword.classList.toggle("fa-eye-slash");
      });

      // 3. Password Strength Indicator
      passwordInput.addEventListener("input", () => {
        const val = passwordInput.value;
        const bar = document.getElementById("strengthBar");
        const text = document.getElementById("strengthText");
        
        let strength = 0;
        if (val.length >= 8) strength++;
        if (/[A-Z]/.test(val)) strength++;
        if (/[0-9]/.test(val)) strength++;
        if (/[^A-Za-z0-9]/.test(val)) strength++;

        const colors = ["#ef4444", "#f59e0b", "#3b82f6", "#10b981"];
        const labels = ["Weak", "Fair", "Good", "Strong"];

        if (val.length === 0) {
          bar.style.width = "0%";
          text.textContent = "Enter password strength";
        } else {
          const index = Math.max(0, strength - 1);
          bar.style.width = `${(strength / 4) * 100}%`;
          bar.style.backgroundColor = colors[index];
          text.textContent = `Strength: ${labels[index]}`;
          text.style.color = colors[index];
        }
      });

      // 4. Experience Slider Label Update
      expSlider.addEventListener("input", (e) => {
        const yrs = e.target.value;
        let label = "Beginner";
        if (yrs > 3 && yrs <= 7) label = "Intermediate";
        if (yrs > 7) label = "Expert";
        expVal.textContent = `${label} (${yrs} yrs)`;
      });

      // 5. File Drag & Drop Setup
      dropZone.addEventListener("click", () => fileInput.click());
      
      fileInput.addEventListener("change", () => {
        if (fileInput.files.length > 0) {
          fileNameDisplay.textContent = `Selected: ${fileInput.files[0].name}`;
        }
      });

      dropZone.addEventListener("dragover", (e) => {
        e.preventDefault();
        dropZone.style.borderColor = "var(--primary-color)";
      });

      dropZone.addEventListener("dragleave", () => {
        dropZone.style.borderColor = "var(--border-color)";
      });

      dropZone.addEventListener("drop", (e) => {
        e.preventDefault();
        dropZone.style.borderColor = "var(--border-color)";
        if (e.dataTransfer.files.length > 0) {
          fileInput.files = e.dataTransfer.files;
          fileNameDisplay.textContent = `Selected: ${e.dataTransfer.files[0].name}`;
        }
      });

      // 6. Navigation Controls
      nextBtn.addEventListener("click", () => {
        if (validateStep(currentStep)) {
          currentStep++;
          if (currentStep === totalSteps) {
            populateSummary();
          }
          updateUI();
        }
      });

      prevBtn.addEventListener("click", () => {
        currentStep--;
        updateUI();
      });

      function updateUI() {
        // Hide/Show Steps
        steps.forEach((step) => {
          step.classList.remove("active");
          if (parseInt(step.dataset.step) === currentStep) {
            step.classList.add("active");
          }
        });

        // Update Progress Bar
        progressSteps.forEach((step, idx) => {
          const stepNum = idx + 1;
          if (stepNum < currentStep) {
            step.classList.add("completed");
            step.classList.remove("active");
          } else if (stepNum === currentStep) {
            step.classList.add("active");
            step.classList.remove("completed");
          } else {
            step.classList.remove("active", "completed");
          }
        });

        const activePercentage = ((currentStep - 1) / (totalSteps - 1)) * 100;
        progressLine.style.width = `${activePercentage}%`;

        // Update Buttons
        prevBtn.style.display = currentStep === 1 ? "none" : "flex";
        if (currentStep === totalSteps) {
          nextBtn.style.display = "none";
          submitBtn.style.display = "flex";
        } else {
          nextBtn.style.display = "flex";
          submitBtn.style.display = "none";
        }
      }

      // 7. Step Validation Logic
      function validateStep(step) {
        let valid = true;
        const currentFormStep = document.querySelector(`.form-step[data-step="${step}"]`);
        const inputs = currentFormStep.querySelectorAll("input[required], select[required]");

        inputs.forEach((input) => {
          const parent = input.closest(".form-group") || input.parentElement;
          
          if (input.type === "checkbox" && !input.checked) {
            parent.classList.add("has-error");
            valid = false;
          } else if (!input.value.trim()) {
            parent.classList.add("has-error");
            valid = false;
          } else if (input.type === "email" && !validateEmail(input.value)) {
            parent.classList.add("has-error");
            valid = false;
          } else if (input.id === "confirmPassword") {
            const pass = document.getElementById("password").value;
            if (input.value !== pass) {
              parent.classList.add("has-error");
              valid = false;
            } else {
              parent.classList.remove("has-error");
            }
          } else if (input.id === "dob" && !validateAge(input.value)) {
            parent.classList.add("has-error");
            valid = false;
          } else {
            parent.classList.remove("has-error");
          }
        });

        return valid;
      }

      function validateEmail(email) {
        return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
      }

      function validateAge(dob) {
        const birthDate = new Date(dob);
        const today = new Date();
        let age = today.getFullYear() - birthDate.getFullYear();
        const m = today.getMonth() - birthDate.getMonth();
        if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())) {
          age--;
        }
        return age >= 18;
      }

      // 8. Populate Summary Screen
      function populateSummary() {
        const summaryBox = document.getElementById("summaryContent");
        const accountType = document.querySelector('input[name="accountType"]:checked').value;
        const username = document.getElementById("username").value;
        const email = document.getElementById("email").value;
        const firstName = document.getElementById("firstName").value;
        const lastName = document.getElementById("lastName").value;
        const country = document.getElementById("country").value;

        // Interests checkboxes
        const interests = Array.from(document.querySelectorAll('input[name="interests"]:checked'))
          .map(cb => cb.value)
          .join(", ") || "None selected";

        summaryBox.innerHTML = `
          <div class="summary-row"><strong>Account Type:</strong> <span>${accountType}</span></div>
          <div class="summary-row"><strong>Full Name:</strong> <span>${firstName} ${lastName}</span></div>
          <div class="summary-row"><strong>Username:</strong> <span>${username}</span></div>
          <div class="summary-row"><strong>Email:</strong> <span>${email}</span></div>
          <div class="summary-row"><strong>Country:</strong> <span>${country}</span></div>
          <div class="summary-row"><strong>Interests:</strong> <span>${interests}</span></div>
        `;
      }

      // 9. Form Submission Event
      form.addEventListener("submit", (e) => {
        e.preventDefault();
        
        const termsCheckbox = document.getElementById("terms");
        if (!termsCheckbox.checked) {
          termsCheckbox.closest(".form-group").classList.add("has-error");
          return;
        }

        // Hide Form Elements & Show Success Banner
        form.style.display = "none";
        document.querySelector(".progressbar").style.display = "none";
        document.getElementById("form-title").textContent = "Success";
        document.getElementById("form-subtitle").textContent = "Your registration details have been submitted.";
        document.getElementById("successScreen").style.display = "block";
      });
    });
  </script>
</body>
</html>
