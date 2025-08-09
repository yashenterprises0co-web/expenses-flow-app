# expenses-flow-app
"Financial Management App"
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Expenses Flow</title>
    
    <!-- Strict DOCTYPE and standards mode -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    
    <!-- Icons: Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    
    <!-- Google Fonts for modern UI -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    
    <!-- Google Identity Services (for OAuth) -->
    <script src="https://accounts.google.com/gsi/client" async defer></script>

    <!-- #################### CSS STYLES #################### -->
    <style>
        /* --- 1. Core Styles & Theming --- */
        :root {
            --primary-color: #007bff;
            --primary-hover: #0056b3;
            --secondary-color: #6c757d;
            --success-color: #28a745;
            --danger-color: #dc3545;
            --warning-color: #ffc107;
            --info-color: #17a2b8;
            --light-color: #f8f9fa;
            --dark-color: #343a40;
            --text-color: #212529;
            --bg-color: #ffffff;
            --border-color: #dee2e6;
            --card-bg: #ffffff;
            --card-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
            --font-family: 'Inter', sans-serif;
            --border-radius: 8px;
        }

        [data-theme="dark"] {
            --primary-color: #0d6efd;
            --primary-hover: #3385fd;
            --text-color: #e9ecef;
            --bg-color: #121212;
            --border-color: #495057;
            --card-bg: #1e1e1e;
            --card-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
            --dark-color: #adb5bd;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        html, body {
            height: 100%;
            font-family: var(--font-family);
            background-color: var(--bg-color);
            color: var(--text-color);
            line-height: 1.6;
            transition: background-color 0.3s, color 0.3s;
        }
        
        /* --- 2. Layout --- */
        #app-container {
            display: flex;
            height: 100vh;
        }

        .sidebar {
            width: 250px;
            background-color: var(--card-bg);
            border-right: 1px solid var(--border-color);
            padding: 1.5rem 1rem;
            display: flex;
            flex-direction: column;
            transition: width 0.3s, transform 0.3s;
        }
        
        .main-content {
            flex-grow: 1;
            padding: 2rem;
            overflow-y: auto;
            background-color: var(--light-color);
        }
        
        [data-theme="dark"] .main-content {
            background-color: #0a0a0a;
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 2rem;
            padding-bottom: 1rem;
            border-bottom: 1px solid var(--border-color);
        }
        
        /* --- 3. Components --- */
        .btn {
            padding: 0.75rem 1.25rem;
            border: none;
            border-radius: var(--border-radius);
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: background-color 0.2s, transform 0.1s;
        }

        .btn-primary {
            background-color: var(--primary-color);
            color: white;
        }

        .btn-primary:hover {
            background-color: var(--primary-hover);
        }

        .btn:active {
            transform: scale(0.98);
        }

        .card {
            background-color: var(--card-bg);
            border-radius: var(--border-radius);
            box-shadow: var(--card-shadow);
            padding: 1.5rem;
            margin-bottom: 1.5rem;
        }

        .card-header {
            font-size: 1.25rem;
            font-weight: 600;
            margin-bottom: 1rem;
        }
        
        .form-group {
            margin-bottom: 1rem;
        }

        .form-group label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 500;
        }

        .form-control {
            width: 100%;
            padding: 0.75rem;
            border: 1px solid var(--border-color);
            border-radius: var(--border-radius);
            background-color: var(--bg-color);
            color: var(--text-color);
            font-size: 1rem;
        }
        
        .form-control:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.25);
        }

        .grid-container {
            display: grid;
            gap: 1.5rem;
        }

        /* --- 4. Navigation & Header --- */
        .sidebar-header {
            font-size: 1.5rem;
            font-weight: 700;
            color: var(--primary-color);
            margin-bottom: 2rem;
            text-align: center;
        }

        .nav-menu {
            list-style: none;
            flex-grow: 1;
        }

        .nav-item a {
            display: flex;
            align-items: center;
            padding: 0.8rem 1rem;
            color: var(--secondary-color);
            text-decoration: none;
            border-radius: var(--border-radius);
            margin-bottom: 0.5rem;
            font-weight: 500;
        }
        
        .nav-item a.active, .nav-item a:hover {
            background-color: var(--primary-color);
            color: white;
        }

        .nav-item i {
            width: 30px;
            font-size: 1.1rem;
        }

        .user-profile {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .user-profile img {
            width: 40px;
            height: 40px;
            border-radius: 50%;
        }

        /* --- 5. Login Screen --- */
        #login-screen {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            flex-direction: column;
            text-align: center;
        }
        
        #login-screen .card {
            width: 100%;
            max-width: 400px;
        }

        /* --- 6. Tables --- */
        .table-container {
            overflow-x: auto;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 1rem;
        }
        
        th, td {
            padding: 0.9rem 1rem;
            text-align: left;
            border-bottom: 1px solid var(--border-color);
        }
        
        thead {
            background-color: var(--light-color);
            font-weight: 600;
        }
        
        [data-theme="dark"] thead {
            background-color: #2c2c2c;
        }
        
        tbody tr:hover {
            background-color: rgba(0, 123, 255, 0.05);
        }
        
        .action-buttons button {
            background: none;
            border: none;
            cursor: pointer;
            font-size: 1.1rem;
            margin: 0 5px;
            color: var(--secondary-color);
        }
        .action-buttons button:hover { color: var(--primary-color); }
        .action-buttons .delete-btn:hover { color: var(--danger-color); }

        /* --- 7. Modal --- */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.6);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        
        .modal-content {
            background-color: var(--card-bg);
            padding: 2rem;
            border-radius: var(--border-radius);
            width: 90%;
            max-width: 800px;
            max-height: 90vh;
            overflow-y: auto;
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1.5rem;
        }

        .modal-header h2 {
            font-size: 1.5rem;
        }

        .close-modal {
            background: none;
            border: none;
            font-size: 1.8rem;
            cursor: pointer;
            color: var(--secondary-color);
        }
        
        /* --- 8. Helper & Utility Classes --- */
        .hidden { display: none !important; }
        .d-flex { display: flex; }
        .justify-between { justify-content: space-between; }
        .align-center { align-items: center; }
        .text-right { text-align: right; }
        .mt-2 { margin-top: 2rem; }
        .mb-1 { margin-bottom: 1rem; }
        .w-50 { width: 50%; }
        .p-1 { padding: 1rem; }
        .gap-1 { gap: 1rem; }

        /* --- 9. Responsive Design --- */
        @media (max-width: 768px) {
            #app-container {
                flex-direction: column;
            }
            .sidebar {
                width: 100%;
                height: auto;
                flex-direction: row;
                justify-content: space-between;
                align-items: center;
                padding: 0.5rem 1rem;
                border-right: none;
                border-bottom: 1px solid var(--border-color);
            }
            .sidebar-header {
                font-size: 1.2rem;
                margin-bottom: 0;
            }
            .nav-menu {
                display: flex;
                flex-grow: 0;
            }
            .nav-item a {
                padding: 0.5rem;
                margin: 0 0.2rem;
            }
            .nav-item span {
                display: none; /* Hide text on mobile nav */
            }
            .nav-item i {
                width: auto;
                font-size: 1.3rem;
            }
            .user-profile-sidebar {
                display: none;
            }
            .main-content {
                padding: 1rem;
            }
            .header {
                margin-bottom: 1rem;
            }
            .grid-container {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>

    <!-- #################### LOGIN SCREEN #################### -->
    <div id="login-screen">
        <div class="card">
            <h1 class="sidebar-header" id="login-app-name">Expenses Flow</h1>
            <p class="mb-1">Your complete financial management solution.</p>
            <div id="g_id_onload"
                 data-client_id="48603052582-hval44o4f6mq3mdh3c2s97vd71ad1ug3.apps.googleusercontent.com"
                 data-callback="handleCredentialResponse"
                 data-context="signin"
                 data-ux_mode="redirect"
                 data-auto_select="true"
                 data-itp_support="true">
            </div>
            <div class="g_id_signin"
                 data-type="standard"
                 data-shape="rectangular"
                 data-theme="outline"
                 data-text="signin_with"
                 data-size="large"
                 data-logo_alignment="left">
            </div>
        </div>
    </div>
    
    <!-- #################### MAIN APPLICATION #################### -->
    <div id="app-container" class="hidden">
        <!-- Sidebar Navigation -->
        <nav class="sidebar">
            <div>
                <h1 class="sidebar-header" id="sidebar-app-name">Expenses Flow</h1>
                <ul class="nav-menu">
                    <li class="nav-item"><a href="#dashboard" class="nav-link active"><i class="fas fa-tachometer-alt"></i> <span>Dashboard</span></a></li>
                    <li class="nav-item"><a href="#documents" class="nav-link"><i class="fas fa-file-alt"></i> <span>Documents</span></a></li>
                    <li class="nav-item"><a href="#admin" class="nav-link"><i class="fas fa-user-shield"></i> <span>Admin</span></a></li>
                    <li class="nav-item"><a href="#settings" class="nav-link"><i class="fas fa-cog"></i> <span>Settings</span></a></li>
                </ul>
            </div>
            <div class="user-profile-sidebar">
                <button id="logout-btn" class="btn" style="width: 100%; background-color: var(--danger-color); color: white;">
                    <i class="fas fa-sign-out-alt"></i> Logout
                </button>
            </div>
        </nav>
        
        <!-- Main Content Area -->
        <main class="main-content">
            <header class="header">
                <h2 id="page-title">Dashboard</h2>
                <div class="user-profile" id="user-profile-header">
                    <!-- User info will be injected here by JS -->
                </div>
            </header>
            
            <div id="view-content">
                <!-- All views will be rendered here -->
            </div>
        </main>
    </div>
    
    <!-- #################### MODAL CONTAINER #################### -->
    <div id="modal-container" class="hidden">
        <div class="modal-overlay">
            <div class="modal-content">
                <!-- Modal content will be injected here -->
            </div>
        </div>
    </div>

<!-- #################### JAVASCRIPT #################### -->
<script>
'use strict';

/**
 * =================================================================
 * CONFIGURATION OBJECT
 * Central place for all application settings.
 * =================================================================
 */
const CONFIG = {
    // Basic Info
    appName: "Expenses Flow",
    companyName: "My Business",
    companyAddress: "123 Business St, Financial District, Mumbai, Maharashtra 400001",
    companyGSTIN: "27AAPCU0000A1Z5", // Sample GSTIN
    
    // Localization
    currency: "INR",  // INR/USD/EUR
    language: "en",   // en/hi/mr (English, Hindi, Marathi)
    dateFormat: "dd-mm-yyyy",
    
    // Invoice Settings
    invoicePrefix: "INV",
    quotationPrefix: "QUO",
    challanPrefix: "CHL",
    invoiceDigits: 5,
    defaultTaxRate: 18,
    
    // Features
    enableGST: true,
    enableWhatsAppSharing: true,

    // Backend
    // ❗️ REPLACE THIS WITH YOUR GOOGLE APPS SCRIPT WEB APP URL
    backendUrl: "https://script.google.com/macros/s/AKfycbw-_q-99e3KiayxCy6aGk8ZZFWtvlZ6mvAszvKuBck4V_1r70xo0U1t_sUSFxg8wrWaWA/exec", 
};

// Global state
const AppState = {
    currentUser: null,
    isAdmin: false,
    currentView: 'dashboard',
    documents: [],
    users: [],
    logs: [],
};


/**
 * =================================================================
 * MAIN APP CLASS
 * The orchestrator of the entire application.
 * =================================================================
 */
class App {
    constructor() {
        this.ui = new UI();
        this.api = new API(CONFIG.backendUrl);
        this.auth = new Auth(this.onLoginSuccess.bind(this), this.onLogout.bind(this));
        this.utils = new Utils();
    }

    init() {
        this.ui.setAppName(CONFIG.appName);
        this.setupEventListeners();
        // The Google Auth script will trigger the login flow
    }

    onLoginSuccess(googleUser) {
        AppState.currentUser = {
            name: googleUser.name,
            email: googleUser.email,
            picture: googleUser.picture,
        };
        // In a real app, you'd verify the token with the backend
        // and get user roles (like isAdmin). For this demo, we'll
        // assume the first user or a specific email is admin.
        this.api.post('login', AppState.currentUser).then(response => {
            if (response.success) {
                AppState.isAdmin = response.data.isAdmin;
                this.ui.showApp();
                this.ui.updateUserInfo(AppState.currentUser);
                this.navigateTo('dashboard');
            } else {
                alert('Login failed: ' + response.message);
                this.auth.signOut();
            }
        });
    }

    onLogout() {
        AppState.currentUser = null;
        AppState.isAdmin = false;
        this.ui.showLogin();
    }
    
    setupEventListeners() {
        // Navigation
        document.querySelector('.nav-menu').addEventListener('click', (e) => {
            e.preventDefault();
            const link = e.target.closest('.nav-link');
            if (link) {
                const view = link.getAttribute('href').substring(1);
                this.navigateTo(view);
            }
        });
        
        // Logout
        document.getElementById('logout-btn').addEventListener('click', () => {
            this.auth.signOut();
        });

        // Event delegation for dynamic content (forms, buttons in tables, etc.)
        document.body.addEventListener('click', (e) => {
            const target = e.target;

            // Create buttons on dashboard
            if (target.matches('[data-action="create"]')) {
                const type = target.dataset.type; // 'invoice', 'quotation', 'challan'
                this.ui.showFormModal(type);
            }
            
            // Document action buttons (view, edit, delete, share, print)
            if (target.closest('[data-doc-action]')) {
                const button = target.closest('[data-doc-action]');
                const action = button.dataset.docAction;
                const id = button.dataset.id;
                this.handleDocumentAction(action, id);
            }
        });
        
        // Modal form submissions
        document.body.addEventListener('submit', (e) => {
            if (e.target.id === 'document-form') {
                e.preventDefault();
                this.handleFormSubmit(e.target);
            }
        });
    }
    
    navigateTo(view) {
        if (!AppState.isAdmin && view === 'admin') {
            alert('Access Denied: Admin only.');
            return;
        }
        
        AppState.currentView = view;
        this.ui.setActiveLink(view);
        this.ui.setPageTitle(view.charAt(0).toUpperCase() + view.slice(1));
        
        switch (view) {
            case 'dashboard':
                this.ui.renderDashboard();
                break;
            case 'documents':
                this.loadAndRenderDocuments();
                break;
            case 'admin':
                this.loadAndRenderAdmin();
                break;
            case 'settings':
                this.ui.renderSettings();
                break;
        }
    }
    
    async loadAndRenderDocuments() {
        try {
            this.ui.showLoading();
            const response = await this.api.get('getDocuments');
            if(response.success) {
                AppState.documents = response.data;
                this.ui.renderDocuments(AppState.documents);
            } else {
                throw new Error(response.message);
            }
        } catch (error) {
            this.ui.renderError('Failed to load documents: ' + error.message);
        }
    }

    async loadAndRenderAdmin() {
        try {
            this.ui.showLoading();
            const [usersRes, logsRes] = await Promise.all([
                this.api.get('getUsers'),
                this.api.get('getLogs')
            ]);
            
            if (usersRes.success) AppState.users = usersRes.data;
            if (logsRes.success) AppState.logs = logsRes.data;

            this.ui.renderAdminPanel(AppState.users, AppState.logs);
        } catch (error) {
            this.ui.renderError('Failed to load admin data: ' + error.message);
        }
    }
    
    handleDocumentAction(action, id) {
        const doc = AppState.documents.find(d => d.id === id);
        if (!doc) return;

        switch(action) {
            case 'view':
            case 'print':
                this.ui.showDocumentDetail(doc, action);
                break;
            case 'edit':
                 // Check if the document was created today
                const createdDate = new Date(doc.date);
                const today = new Date();
                if (createdDate.toDateString() === today.toDateString() || AppState.isAdmin) {
                    this.ui.showFormModal(doc.type, doc);
                } else {
                    alert("Editing is only allowed for documents created today or by an admin.");
                }
                break;
            case 'delete':
                if (confirm(`Are you sure you want to delete ${doc.type} #${doc.number}?`)) {
                    this.api.post('deleteDocument', { id }).then(res => {
                        if (res.success) {
                            alert('Document deleted successfully.');
                            this.loadAndRenderDocuments();
                        } else {
                            alert('Error: ' + res.message);
                        }
                    });
                }
                break;
            case 'share':
                this.ui.showShareOptions(doc);
                break;
        }
    }
    
    async handleFormSubmit(form) {
        const formData = new FormData(form);
        const data = Object.fromEntries(formData.entries());
        data.items = this.ui.getFormItems(); // Special handling for items
        
        // Basic Validation
        if (!data.clientName || !data.date) {
            alert('Client Name and Date are required.');
            return;
        }

        const endpoint = data.id ? 'updateDocument' : 'createDocument';
        
        try {
            const response = await this.api.post(endpoint, data);
            if (response.success) {
                alert(`${data.type} ${data.id ? 'updated' : 'created'} successfully!`);
                this.ui.hideModal();
                this.loadAndRenderDocuments();
            } else {
                throw new Error(response.message);
            }
        } catch (error) {
            alert('Submission failed: ' + error.message);
        }
    }
}


/**
 * =================================================================
 * AUTHENTICATION CLASS
 * Handles Google Sign-In and Sign-Out.
 * =================================================================
 */
class Auth {
    constructor(onLoginSuccess, onLogout) {
        this.onLoginSuccess = onLoginSuccess;
        this.onLogout = onLogout;
        window.handleCredentialResponse = this.handleCredentialResponse.bind(this);
    }
    
    handleCredentialResponse(response) {
        // Decode JWT to get user profile info without backend verification
        const decodedToken = this.decodeJwtResponse(response.credential);
        this.onLoginSuccess(decodedToken);
    }

    decodeJwtResponse(token) {
        var base64Url = token.split('.')[1];
        var base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
        var jsonPayload = decodeURIComponent(atob(base64).split('').map(function(c) {
            return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
        }).join(''));
        return JSON.parse(jsonPayload);
    }

    signOut() {
        google.accounts.id.disableAutoSelect();
        // Here you might call a backend endpoint to invalidate a session
        this.onLogout();
    }
}


/**
 * =================================================================
 * API SERVICE CLASS
 * Manages all communication with the Google Apps Script backend.
 * =================================================================
 */
class API {
    constructor(baseUrl) {
        this.baseUrl = baseUrl;
    }
    
    async request(action, method = 'GET', body = null) {
        this.ui = this.ui || new UI(); // Lazy load UI
        this.ui.showLoadingSpinner();
        
        const options = {
            method: 'POST', // GAS Web Apps work best with POST, even for GET actions
            headers: {
                'Content-Type': 'text/plain;charset=utf-8', // Required for GAS
            },
            body: JSON.stringify({
                action,
                payload: body,
                user: AppState.currentUser, // Send user info with every request
            }),
            redirect: 'follow',
        };

        try {
            const response = await fetch(this.baseUrl, options);
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            return await response.json();
        } catch (error) {
            console.error('API Error:', error);
            return { success: false, message: error.message };
        } finally {
            this.ui.hideLoadingSpinner();
        }
    }
    
    async get(action, params = {}) {
        // GAS wrapper for GET
        return this.request(action, 'GET', params);
    }

    async post(action, data) {
        return this.request(action, 'POST', data);
    }
}


/**
 * =================================================================
 * UI CLASS
 * Handles all DOM manipulations and rendering.
 * =================================================================
 */
class UI {
    constructor() {
        this.appContainer = document.getElementById('app-container');
        this.loginScreen = document.getElementById('login-screen');
        this.viewContent = document.getElementById('view-content');
        this.pageTitle = document.getElementById('page-title');
        this.modalContainer = document.getElementById('modal-container');
        this.utils = new Utils();
    }

    setAppName(name) {
        document.getElementById('login-app-name').textContent = name;
        document.getElementById('sidebar-app-name').textContent = name;
        document.title = name;
    }

    showApp() {
        this.appContainer.classList.remove('hidden');
        this.loginScreen.classList.add('hidden');
    }

    showLogin() {
        this.appContainer.classList.add('hidden');
        this.loginScreen.classList.remove('hidden');
    }

    updateUserInfo(user) {
        const profileHTML = `
            <div class="text-right">
                <span style="font-weight: 500;">${user.name}</span><br>
                <small>${user.email}</small>
            </div>
            <img src="${user.picture}" alt="${user.name}">
        `;
        document.getElementById('user-profile-header').innerHTML = profileHTML;
    }

    setActiveLink(view) {
        document.querySelectorAll('.nav-link').forEach(link => {
            link.classList.remove('active');
            if (link.getAttribute('href') === `#${view}`) {
                link.classList.add('active');
            }
        });
    }

    setPageTitle(title) {
        this.pageTitle.textContent = title;
    }

    showLoading() {
        this.viewContent.innerHTML = `<div class="card"><i class="fas fa-spinner fa-spin"></i> Loading...</div>`;
    }

    renderError(message) {
        this.viewContent.innerHTML = `<div class="card" style="border-left: 5px solid var(--danger-color);">${message}</div>`;
    }

    renderDashboard() {
        const html = `
            <div class="grid-container" style="grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));">
                <div class="card">
                    <div class="card-header">Quick Actions</div>
                    <button class="btn btn-primary mb-1" style="width:100%" data-action="create" data-type="invoice">
                        <i class="fas fa-plus"></i> Create Invoice
                    </button>
                    <button class="btn" style="width:100%; background-color: var(--info-color); color:white;" data-action="create" data-type="quotation">
                        <i class="fas fa-plus"></i> Create Quotation
                    </button>
                    <button class="btn mt-2" style="width:100%; background-color: var(--secondary-color); color:white;" data-action="create" data-type="challan">
                        <i class="fas fa-plus"></i> Create Challan
                    </button>
                </div>
                 <div class="card">
                    <div class="card-header">This Month's Revenue</div>
                    <h2 style="font-size: 2rem; color: var(--success-color);">${this.utils.formatCurrency(125000)}</h2>
                    <p>vs ${this.utils.formatCurrency(98000)} last month</p>
                </div>
                <div class="card">
                    <div class="card-header">Pending Invoices</div>
                    <h2 style="font-size: 2rem; color: var(--warning-color);">5</h2>
                    <p>Totaling ${this.utils.formatCurrency(45000)}</p>
                </div>
            </div>
        `;
        this.viewContent.innerHTML = html;
    }
    
    renderDocuments(docs) {
        const tableHtml = this.generateTable(docs, [
            { key: 'number', label: 'Number' },
            { key: 'type', label: 'Type' },
            { key: 'clientName', label: 'Client' },
            { key: 'date', label: 'Date', format: (d) => this.utils.formatDate(d) },
            { key: 'total', label: 'Amount', format: (v) => this.utils.formatCurrency(v) },
            { key: 'status', label: 'Status' }
        ]);

        this.viewContent.innerHTML = `
            <div class="card">
                <div class="card-header">All Documents</div>
                <div class="table-container">${tableHtml}</div>
            </div>
        `;
    }
    
    renderAdminPanel(users, logs) {
        const usersTable = this.generateTable(users, [
            { key: 'name', label: 'Name' },
            { key: 'email', label: 'Email' },
            { key: 'role', label: 'Role' },
        ], 'admin');
        
        const logsTable = this.generateTable(logs.slice(0,10), [ // show last 10 logs
            { key: 'timestamp', label: 'Timestamp', format: (ts) => new Date(ts).toLocaleString() },
            { key: 'user', label: 'User' },
            { key: 'action', label: 'Action' }
        ]);

        this.viewContent.innerHTML = `
            <div class="card">
                <div class="card-header d-flex justify-between align-center">
                    <span>User Management</span>
                    <button class="btn btn-primary"><i class="fas fa-plus"></i> Add User</button>
                </div>
                <div class="table-container">${usersTable}</div>
            </div>
            <div class="card">
                <div class="card-header">Activity Logs</div>
                <div class="table-container">${logsTable}</div>
            </div>
        `;
    }

    renderSettings() {
        this.viewContent.innerHTML = `
            <div class="grid-container" style="grid-template-columns: 1fr 1fr;">
                <div class="card">
                    <div class="card-header">Company Profile</div>
                    <form id="company-profile-form">
                        <div class="form-group">
                            <label for="companyName">Company Name</label>
                            <input type="text" id="companyName" class="form-control" value="${CONFIG.companyName}">
                        </div>
                        <div class="form-group">
                            <label for="companyAddress">Address</label>
                            <textarea id="companyAddress" class="form-control">${CONFIG.companyAddress}</textarea>
                        </div>
                        <div class="form-group">
                            <label for="companyGstin">GSTIN</label>
                            <input type="text" id="companyGstin" class="form-control" value="${CONFIG.companyGSTIN}">
                        </div>
                        <button type="submit" class="btn btn-primary">Save Changes</button>
                    </form>
                </div>
                <div class="card">
                    <div class="card-header">Customization</div>
                    <div class="form-group">
                        <label>Theme</label>
                        <div>
                            <input type="radio" id="theme-light" name="theme" value="light" checked>
                            <label for="theme-light">Light</label>
                            <input type="radio" id="theme-dark" name="theme" value="dark">
                            <label for="theme-dark">Dark</label>
                        </div>
                    </div>
                     <div class="form-group">
                        <label for="language-select">Language</label>
                        <select id="language-select" class="form-control">
                            <option value="en">English</option>
                            <option value="hi">Hindi (हिंदी)</option>
                            <option value="mr">Marathi (मराठी)</option>
                        </select>
                    </div>
                </div>
            </div>
        `;
        // Add event listeners for settings
        const themeDark = document.getElementById('theme-dark');
        const themeLight = document.getElementById('theme-light');
        const currentTheme = localStorage.getItem('theme') || 'light';
        document.documentElement.setAttribute('data-theme', currentTheme);
        if (currentTheme === 'dark') themeDark.checked = true;
        
        themeDark.addEventListener('change', () => {
            document.documentElement.setAttribute('data-theme', 'dark');
            localStorage.setItem('theme', 'dark');
        });
        themeLight.addEventListener('change', () => {
            document.documentElement.setAttribute('data-theme', 'light');
            localStorage.setItem('theme', 'light');
        });
    }

    generateTable(data, columns, type = 'document') {
        if (!data || data.length === 0) {
            return '<p>No data available.</p>';
        }

        const headers = columns.map(col => `<th>${col.label}</th>`).join('') + (type === 'document' ? '<th>Actions</th>' : '');
        
        const rows = data.map(row => {
            const cells = columns.map(col => {
                const value = row[col.key] || '';
                return `<td>${col.format ? col.format(value) : value}</td>`;
            }).join('');
            
            const actions = type === 'document' ? `
                <td class="action-buttons">
                    <button title="View/Print" data-doc-action="view" data-id="${row.id}"><i class="fas fa-eye"></i></button>
                    <button title="Edit" data-doc-action="edit" data-id="${row.id}"><i class="fas fa-edit"></i></button>
                    <button title="Share" data-doc-action="share" data-id="${row.id}"><i class="fas fa-share-alt"></i></button>
                    <button title="Delete" data-doc-action="delete" data-id="${row.id}" class="delete-btn"><i class="fas fa-trash"></i></button>
                </td>
            ` : '';
            
            return `<tr>${cells}${actions}</tr>`;
        }).join('');
        
        return `<table><thead><tr>${headers}</tr></thead><tbody>${rows}</tbody></table>`;
    }

    showModal(title, content) {
        this.modalContainer.innerHTML = `
            <div class="modal-overlay">
                <div class="modal-content">
                    <div class="modal-header">
                        <h2>${title}</h2>
                        <button class="close-modal" onclick="app.ui.hideModal()">&times;</button>
                    </div>
                    ${content}
                </div>
            </div>
        `;
        this.modalContainer.classList.remove('hidden');
    }

    hideModal() {
        this.modalContainer.classList.add('hidden');
        this.modalContainer.innerHTML = '';
    }
    
    showLoadingSpinner() { /* Add a small spinner to a corner of the screen */ }
    hideLoadingSpinner() { /* Hide the spinner */ }
    
    showFormModal(type, data = {}) {
        const isEditing = !!data.id;
        const title = `${isEditing ? 'Edit' : 'Create'} ${type.charAt(0).toUpperCase() + type.slice(1)}`;
        
        const formContent = `
            <form id="document-form">
                <input type="hidden" name="id" value="${data.id || ''}">
                <input type="hidden" name="type" value="${type}">
                
                <div class="grid-container" style="grid-template-columns: 1fr 1fr 1fr; gap:1.5rem">
                    <div class="form-group">
                        <label>${type.charAt(0).toUpperCase() + type.slice(1)} Number</label>
                        <input type="text" name="number" class="form-control" value="${data.number || 'Auto-generated'}" readonly>
                    </div>
                    <div class="form-group">
                        <label>Date</label>
                        <input type="date" name="date" class="form-control" value="${data.date || new Date().toISOString().split('T')[0]}" required>
                    </div>
                    <div class="form-group">
                        <label>Due Date</label>
                        <input type="date" name="dueDate" class="form-control" value="${data.dueDate || ''}">
                    </div>
                </div>

                <div class="card mb-1">
                    <div class="card-header">Client Details</div>
                    <div class="grid-container" style="grid-template-columns: 1fr 1fr 1fr; gap:1.5rem">
                        <div class="form-group">
                            <label for="clientName">Client Name</label>
                            <input type="text" id="clientName" name="clientName" class="form-control" value="${data.clientName || ''}" required>
                        </div>
                        <div class="form-group">
                            <label for="clientPhone">Contact Number (India)</label>
                            <input type="tel" id="clientPhone" name="clientPhone" class="form-control" pattern="[6-9][0-9]{9}" title="10-digit Indian mobile number" value="${data.clientPhone || ''}">
                        </div>
                        ${CONFIG.enableGST ? `
                        <div class="form-group">
                            <label for="clientGstin">Client GSTIN</label>
                            <input type="text" id="clientGstin" name="clientGstin" class="form-control" maxlength="15" pattern="^[0-9]{2}[A-Z]{5}[0-9]{4}[A-Z]{1}[1-9A-Z]{1}Z[0-9A-Z]{1}$" title="Valid 15-character GSTIN" value="${data.clientGstin || ''}">
                        </div>` : ''}
                    </div>
                </div>
                
                <div class="card mb-1">
                    <div class="card-header">Items</div>
                    <div id="items-container">
                        <!-- Items will be injected here -->
                    </div>
                    <button type="button" id="add-item-btn" class="btn" style="background-color: var(--secondary-color); color:white;"><i class="fas fa-plus"></i> Add Item</button>
                </div>

                <div class="card">
                     <div class="grid-container" style="grid-template-columns: 1fr 1fr; gap:1.5rem">
                        <div>
                            <div class="form-group">
                                <label for="notes">Notes</label>
                                <textarea id="notes" name="notes" class="form-control">${data.notes || ''}</textarea>
                            </div>
                        </div>
                        <div id="totals-section">
                           <!-- Totals will be injected here -->
                        </div>
                    </div>
                </div>

                <div class="mt-2 text-right">
                    <button type="button" class="btn" style="background:var(--secondary-color); color:white;" onclick="app.ui.hideModal()">Cancel</button>
                    <button type="submit" class="btn btn-primary">${isEditing ? 'Save Changes' : 'Create ' + type}</button>
                </div>
            </form>
        `;
        
        this.showModal(title, formContent);
        this.setupFormInteractivity(data.items || [{}]);
    }

    setupFormInteractivity(items) {
        const itemsContainer = document.getElementById('items-container');
        const addItemBtn = document.getElementById('add-item-btn');
        
        const renderItems = () => {
            itemsContainer.innerHTML = items.map((item, index) => this.getItemRowHTML(item, index)).join('');
        };
        
        const calculateTotals = () => {
            let subtotal = 0;
            const currentItems = this.getFormItems();
            currentItems.forEach(item => {
                subtotal += (item.quantity || 0) * (item.price || 0);
            });
            
            const taxRate = parseFloat(document.getElementById('tax-rate-input')?.value) || CONFIG.defaultTaxRate;
            let cgst = 0, sgst = 0, igst = 0;

            if (CONFIG.enableGST) {
                // Simplified IGST logic: assume IGST if client GSTIN starts with a different state code than company
                const clientGstin = document.getElementById('clientGstin')?.value || '';
                const isInterState = clientGstin && clientGstin.substring(0, 2) !== CONFIG.companyGSTIN.substring(0, 2);

                if (isInterState) {
                    igst = subtotal * (taxRate / 100);
                } else {
                    cgst = subtotal * (taxRate / 2 / 100);
                    sgst = subtotal * (taxRate / 2 / 100);
                }
            }
            const total = subtotal + cgst + sgst + igst;
            this.renderTotals(subtotal, cgst, sgst, igst, total, taxRate);
        };

        addItemBtn.addEventListener('click', () => {
            items.push({});
            renderItems();
        });

        itemsContainer.addEventListener('click', (e) => {
            if (e.target.classList.contains('remove-item-btn')) {
                const index = e.target.dataset.index;
                items.splice(index, 1);
                if (items.length === 0) items.push({}); // Always have at least one row
                renderItems();
                calculateTotals();
            }
        });

        document.getElementById('document-form').addEventListener('input', (e) => {
             if (e.target.closest('#items-container') || e.target.id === 'tax-rate-input') {
                calculateTotals();
             }
        });

        renderItems();
        calculateTotals();
    }
    
    getFormItems() {
        const items = [];
        const itemRows = document.querySelectorAll('.item-row');
        itemRows.forEach(row => {
            items.push({
                description: row.querySelector('[name="description"]').value,
                hsn: row.querySelector('[name="hsn"]')?.value,
                quantity: parseFloat(row.querySelector('[name="quantity"]').value),
                price: parseFloat(row.querySelector('[name="price"]').value),
            });
        });
        return items.filter(item => item.description); // Only return items with a description
    }

    getItemRowHTML(item, index) {
        return `
            <div class="item-row grid-container mb-1" style="grid-template-columns: 3fr ${CONFIG.enableGST ? '1fr' : ''} 1fr 1fr 1fr auto; align-items:flex-end; gap:1rem;">
                <div class="form-group">
                    ${index === 0 ? '<label>Description</label>' : ''}
                    <input type="text" name="description" class="form-control" placeholder="Item or Service" value="${item.description || ''}">
                </div>
                ${CONFIG.enableGST ? `
                <div class="form-group">
                    ${index === 0 ? '<label>HSN/SAC</label>' : ''}
                    <input type="text" name="hsn" class="form-control" placeholder="e.g. 998314" value="${item.hsn || ''}">
                </div>` : ''}
                <div class="form-group">
                    ${index === 0 ? '<label>Quantity</label>' : ''}
                    <input type="number" name="quantity" class="form-control" placeholder="1" value="${item.quantity || 1}">
                </div>
                <div class="form-group">
                    ${index === 0 ? '<label>Price</label>' : ''}
                    <input type="number" name="price" class="form-control" placeholder="0.00" value="${item.price || ''}">
                </div>
                 <div class="form-group">
                    ${index === 0 ? '<label>Total</label>' : ''}
                    <input type="text" name="item-total" class="form-control" value="${this.utils.formatCurrency((item.quantity || 1) * (item.price || 0))}" readonly>
                </div>
                <button type="button" class="btn remove-item-btn" data-index="${index}" style="height:45px; background-color:var(--danger-color); color:white;">&times;</button>
            </div>
        `;
    }

    renderTotals(subtotal, cgst, sgst, igst, total, taxRate) {
        const totalsSection = document.getElementById('totals-section');
        totalsSection.innerHTML = `
            <div class="d-flex justify-between mb-1"><span>Subtotal:</span> <span>${this.utils.formatCurrency(subtotal)}</span></div>
            ${CONFIG.enableGST ? `
                <div class="d-flex justify-between align-center mb-1">
                    <span>Tax Rate:</span>
                    <input type="number" id="tax-rate-input" step="0.01" class="form-control" style="width: 80px;" value="${taxRate}"> %
                </div>
                ${igst > 0 ? `
                <div class="d-flex justify-between mb-1"><span>IGST:</span> <span>${this.utils.formatCurrency(igst)}</span></div>
                ` : `
                <div class="d-flex justify-between mb-1"><span>CGST:</span> <span>${this.utils.formatCurrency(cgst)}</span></div>
                <div class="d-flex justify-between mb-1"><span>SGST:</span> <span>${this.utils.formatCurrency(sgst)}</span></div>
                `}
            ` : ''}
            <hr style="border: 1px solid var(--border-color); margin: 1rem 0;">
            <div class="d-flex justify-between" style="font-size: 1.2rem; font-weight: 600;">
                <span>Total:</span>
                <span>${this.utils.formatCurrency(total)}</span>
            </div>
        `;
    }
    
    showDocumentDetail(doc, mode = 'view') {
        // This generates a printable/viewable version of the invoice
        const isPrint = mode === 'print';
        
        let detailHTML = `
            <style>
              .invoice-box { max-width: 800px; margin: auto; padding: 20px; border: 1px solid #eee; box-shadow: 0 0 10px rgba(0, 0, 0, 0.15); font-size: 16px; line-height: 24px; font-family: 'Helvetica Neue', 'Helvetica', sans-serif; color: #555; }
              /* ... add more print-specific styles here */
            </style>
            <div class="invoice-box">
                <h2>${doc.type.toUpperCase()}</h2>
                <p>Number: ${doc.number}</p>
                <p>Date: ${this.utils.formatDate(doc.date)}</p>
                <!-- Add all other details from 'doc' object -->
                <p>Client: ${doc.clientName}</p>
                <h3>Total: ${this.utils.formatCurrency(doc.total)}</h3>
            </div>
        `;
        
        if (isPrint) {
            const printWindow = window.open('', '_blank');
            printWindow.document.write(`<html><head><title>Print ${doc.type}</title>${detailHTML}</head><body>`);
            printWindow.document.close();
            printWindow.focus();
            printWindow.print();
            printWindow.close();
        } else {
             this.showModal(`${doc.type} Details`, detailHTML + `<br><button class="btn btn-primary" onclick="app.handleDocumentAction('print', '${doc.id}')">Print</button>`);
        }
    }

    showShareOptions(doc) {
        const text = `Check out this ${doc.type} (#${doc.number}) for ${doc.clientName} - Total: ${this.utils.formatCurrency(doc.total)}. View: ${window.location.href}`;
        const whatsappUrl = `https://api.whatsapp.com/send?text=${encodeURIComponent(text)}`;
        const emailUrl = `mailto:?subject=${doc.type} #${doc.number}&body=${encodeURIComponent(text)}`;

        const content = `
            <h3>Share ${doc.type} #${doc.number}</h3>
            <a href="${whatsappUrl}" target="_blank" class="btn btn-primary" style="background-color:#25D366"><i class="fab fa-whatsapp"></i> Share on WhatsApp</a>
            <a href="${emailUrl}" class="btn btn-primary" style="background-color:#EA4335"><i class="fas fa-envelope"></i> Share via Email</a>
        `;
        this.showModal('Share Document', content);
    }
}


/**
 * =================================================================
 * UTILITIES CLASS
 * Helper functions for formatting, validation, etc.
 * =================================================================
 */
class Utils {
    formatCurrency(amount, currency = CONFIG.currency) {
        return new Intl.NumberFormat('en-IN', {
            style: 'currency',
            currency: currency,
        }).format(amount || 0);
    }

    formatDate(dateString) {
        if (!dateString) return '';
        const date = new Date(dateString);
        const options = { day: '2-digit', month: '2-digit', year: 'numeric' };
        // Simple replace to match dd-mm-yyyy format if needed
        return date.toLocaleDateString('en-GB', options).replace(/\//g, '-');
    }
    
    validateGSTIN(gstin) {
        const regex = /^[0-9]{2}[A-Z]{5}[0-9]{4}[A-Z]{1}[1-9A-Z]{1}Z[0-9A-Z]{1}$/;
        return regex.test(gstin);
    }
    
    validateIndianPhone(phone) {
        const regex = /^[6-9]\d{9}$/;
        return regex.test(phone);
    }
}


/**
 * =================================================================
 * APP INITIALIZATION
 * =================================================================
 */
const app = new App();
document.addEventListener('DOMContentLoaded', () => {
    app.init();
});

</script>
</body>
</html>
