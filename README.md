<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Expenses Flow</title>
    
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    
    <script src="https://accounts.google.com/gsi/client" async defer></script>

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
        
        /* --- All other CSS rules --- */
        .hidden {
            display: none !important;
        }
        
        .loading-spinner {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100px;
            font-size: 2rem;
            color: var(--primary-color);
        }

        .card {
            background-color: var(--card-bg);
            border-radius: var(--border-radius);
            box-shadow: var(--card-shadow);
            padding: 1.5rem;
            margin-bottom: 1.5rem;
        }
        
        .card-header {
            font-weight: 600;
            font-size: 1.1rem;
            margin-bottom: 1rem;
            padding-bottom: 0.5rem;
            border-bottom: 1px solid var(--border-color);
        }
        
        /* Flex & Alignment Utilities */
        .d-flex { display: flex; }
        .justify-between { justify-content: space-between; }
        .align-center { align-items: center; }
        .gap-1 { gap: 1rem; }
        .gap-05 { gap: 0.5rem; }
        
        /* Buttons */
        .btn {
            padding: 0.75rem 1.25rem;
            border: none;
            border-radius: var(--border-radius);
            cursor: pointer;
            font-size: 1rem;
            font-weight: 500;
            transition: background-color 0.2s, color 0.2s;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
        }
        
        .btn-primary {
            background-color: var(--primary-color);
            color: #fff;
        }
        
        .btn-primary:hover {
            background-color: var(--primary-hover);
        }
        
        .btn-sm {
            padding: 0.5rem 1rem;
            font-size: 0.875rem;
        }
        
        .btn-icon {
            padding: 0.5rem;
        }
        
        /* Form elements */
        .form-group {
            margin-bottom: 1rem;
        }
        
        label {
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
            transition: border-color 0.2s, box-shadow 0.2s;
        }
        
        .form-control:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.25);
        }
        
        /* Grid Layout */
        .grid-container {
            display: grid;
            gap: 1.5rem;
        }
        
        /* Tables */
        .table-container {
            overflow-x: auto;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
        }
        
        th, td {
            text-align: left;
            padding: 1rem;
            border-bottom: 1px solid var(--border-color);
        }
        
        th {
            background-color: var(--light-color);
            font-weight: 600;
            color: var(--dark-color);
        }
        
        [data-theme="dark"] th {
            background-color: var(--card-bg);
            color: var(--text-color);
        }

        /* Nav Menu */
        .nav-menu {
            list-style: none;
            flex-grow: 1;
            margin-top: 2rem;
        }
        
        .nav-item {
            margin-bottom: 0.5rem;
        }
        
        .nav-link {
            display: flex;
            align-items: center;
            gap: 1rem;
            padding: 0.75rem 1rem;
            text-decoration: none;
            color: var(--dark-color);
            border-radius: var(--border-radius);
            transition: background-color 0.2s, color 0.2s;
        }
        
        .nav-link i {
            width: 20px;
            text-align: center;
        }
        
        .nav-link:hover {
            background-color: rgba(0, 0, 0, 0.05);
        }
        
        .nav-link.active {
            background-color: var(--primary-color);
            color: #fff;
        }
        
        .nav-link.active:hover {
            background-color: var(--primary-hover);
        }

        [data-theme="dark"] .nav-link {
            color: var(--text-color);
        }

        /* User Profile */
        .user-profile-sidebar {
            margin-top: auto;
            padding-top: 1rem;
            border-top: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            gap: 1rem;
        }
        
        .user-profile-sidebar img {
            width: 50px;
            height: 50px;
            border-radius: 50%;
        }

        /* Login Screen */
        #login-screen {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            text-align: center;
        }
        
        #login-screen .card {
            width: 100%;
            max-width: 400px;
            padding: 2rem;
        }

        /* Modals */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
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
            position: relative;
        }
        
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1.5rem;
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 1rem;
        }
        
        .close-modal {
            background: none;
            border: none;
            font-size: 2rem;
            cursor: pointer;
            color: var(--secondary-color);
        }
        
        /* Responsive Design */
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
                margin-top: 0;
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

    <div id="login-screen">
        <div class="card">
            <h1 class="sidebar-header" id="login-app-name">Expenses Flow</h1>
            <p class="mb-1">Your complete financial management solution.</p>
            <div id="g_id_onload"
                 data-client_id="48603052582-hval44o4f6mq3mdh3c2s97vd71ad1ug3.apps.googleusercontent.com"
                 data-callback="handleCredentialResponse"
                 data-context="signin"
                 data-ux_mode="popup"
                 data-auto_select="true"
                 data-itp_support="true"
                 data-use_fedcm_for_prompt="false">
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
    
    <div id="app-container" class="hidden">
        <div class="sidebar">
            <h1 class="sidebar-header" id="sidebar-app-name">Expenses Flow</h1>
            <nav>
                <ul class="nav-menu">
                    <li class="nav-item">
                        <a href="#dashboard" class="nav-link active">
                            <i class="fas fa-home"></i> <span>Dashboard</span>
                        </a>
                    </li>
                    <li class="nav-item">
                        <a href="#documents" class="nav-link">
                            <i class="fas fa-file-alt"></i> <span>Documents</span>
                        </a>
                    </li>
                    <li class="nav-item admin-link">
                        <a href="#admin" class="nav-link">
                            <i class="fas fa-user-shield"></i> <span>Admin</span>
                        </a>
                    </li>
                    <li class="nav-item">
                        <a href="#settings" class="nav-link">
                            <i class="fas fa-cog"></i> <span>Settings</span>
                        </a>
                    </li>
                </ul>
            </nav>
            <div class="user-profile-sidebar">
                <div id="user-profile-sidebar-content"></div>
                <button id="logout-btn" class="btn btn-sm btn-icon"><i class="fas fa-sign-out-alt"></i></button>
            </div>
        </div>
        <div class="main-content">
            <div class="header">
                <h2 id="page-title">Dashboard</h2>
                <div class="d-flex align-center" id="user-profile-header">
                </div>
            </div>
            <div id="view-content">
            </div>
        </div>
    </div>
    <div id="modal-container" class="hidden">
    </div>
    <script>
    'use strict';

    const CONFIG = {
        appName: "Expenses Flow",
        companyName: "Yash Enterprises",
        companyAddress: "123, ABC Road, Pune, Maharashtra, India - 411001",
        companyGSTIN: "27AABBCC1234A1Z5",
        backendUrl: "https://script.google.com/macros/s/AKfycbx_VMXruosNiBKRyjEAOVx7DNta-oT7buUgaHWDUWIGS_wPeIg776YDpluf53x28qisVg/exec", // ❗️ IMPORTANT: PASTE YOUR DEPLOYMENT URL HERE
        adminEmail: "YOUR_EMAIL@gmail.com"
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
        }

        onLoginSuccess(googleUser) {
            AppState.currentUser = {
                name: googleUser.name,
                email: googleUser.email,
                picture: googleUser.picture,
            };
            
            // Check for admin role on the frontend
            AppState.isAdmin = (AppState.currentUser.email === CONFIG.adminEmail);

            this.api.post('login', AppState.currentUser).then(response => {
                if (response.success) {
                    this.ui.showApp();
                    this.ui.updateUserInfo(AppState.currentUser, AppState.isAdmin);
                    this.navigateTo('dashboard');
                } else {
                    alert('Login failed: ' + response.message);
                    this.auth.signOut();
                }
            }).catch(error => {
                alert('An error occurred during login. Please try again.');
                this.auth.signOut();
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

            // Theme toggle (in settings, not shown here)
            document.body.addEventListener('change', (e) => {
                if (e.target.id === 'theme-toggle') {
                    document.documentElement.dataset.theme = e.target.checked ? 'dark' : 'light';
                    localStorage.setItem('theme', document.documentElement.dataset.theme);
                }
            });

            // Event delegation for dynamic content
            document.body.addEventListener('click', (e) => {
                const target = e.target;
                
                // Modal close button
                if (target.matches('.close-modal')) {
                    this.ui.hideModal();
                }
                
                // Close modal on outside click
                if (target.matches('#modal-container')) {
                    this.ui.hideModal();
                }

                // Create buttons on dashboard
                if (target.matches('[data-action="create"]')) {
                    const type = target.dataset.type;
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
                this.ui.renderError('Access Denied: Admin only.');
                return;
            }
            
            AppState.currentView = view;
            this.ui.setActiveLink(view);
            this.ui.setPageTitle(this.utils.capitalize(view));
            
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
                    this.ui.renderError('Failed to load documents: ' + response.message);
                }
            } catch (error) {
                this.ui.renderError('Failed to load documents due to a network error.');
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
                    // Check if the document was created today OR if user is admin
                    const createdDate = new Date(doc.createdAt); // assuming a 'createdAt' property from backend
                    const today = new Date();
                    const isToday = createdDate.toDateString() === today.toDateString();
                    if (isToday || AppState.isAdmin) {
                        this.ui.showFormModal(doc.type, doc);
                    } else {
                        alert("Editing is only allowed for documents created today or for admins.");
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
                        }).catch(() => {
                            alert('Network error while deleting document.');
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
                    alert('Submission failed: ' + response.message);
                }
            } catch (error) {
                alert('Submission failed due to a network error.');
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
            const decodedToken = this.decodeJwtResponse(response.credential);
            this.onLoginSuccess(decodedToken);
        }

        decodeJwtResponse(token) {
            const base64Url = token.split('.')[1];
            const base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
            const jsonPayload = decodeURIComponent(atob(base64).split('').map(function(c) {
                return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
            }).join(''));
            return JSON.parse(jsonPayload);
        }

        signOut() {
            google.accounts.id.disableAutoSelect();
            // A more robust signout would also invalidate server-side session
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
            this.ui = new UI();
        }
        
        async request(action, body = null) {
            this.ui.showLoadingSpinner();
            
            const options = {
                method: 'POST', // GAS Web Apps work best with POST
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
                const data = await response.json();
                if (!data.success) {
                    console.error('Backend Error:', data.message);
                    throw new Error(data.message);
                }
                return data;
            } catch (error) {
                console.error('API Error:', error);
                return { success: false, message: error.message };
            } finally {
                this.ui.hideLoadingSpinner();
            }
        }
        
        async get(action, params = {}) {
            return this.request(action, params);
        }

        async post(action, data) {
            return this.request(action, data);
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

        updateUserInfo(user, isAdmin) {
            const profileHeaderHTML = `
                <div class="d-flex align-center gap-05">
                    <span style="font-weight: 500;">${user.name}</span>
                    <img src="${user.picture}" alt="${user.name}" style="width: 40px; height: 40px; border-radius: 50%;">
                </div>
            `;
            const profileSidebarHTML = `
                <div class="d-flex align-center gap-1">
                    <img src="${user.picture}" alt="${user.name}">
                    <div style="font-weight: 500;">${user.name}<br><small>${isAdmin ? 'Admin' : 'User'}</small></div>
                </div>
            `;
            document.getElementById('user-profile-header').innerHTML = profileHeaderHTML;
            document.getElementById('user-profile-sidebar-content').innerHTML = profileSidebarHTML;
            
            // Show/hide admin link based on role
            const adminLink = document.querySelector('.admin-link');
            if (isAdmin) {
                adminLink.style.display = 'list-item';
            } else {
                adminLink.style.display = 'none';
            }
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
        
        showLoadingSpinner() {
            if (!this.modalContainer.classList.contains('hidden')) return; // Don't show if a modal is open
            this.viewContent.innerHTML = `<div class="loading-spinner"><i class="fas fa-circle-notch fa-spin"></i></div>`;
        }
        
        hideLoadingSpinner() {
            // Can add logic to check if a loading spinner is currently visible
            const spinner = this.viewContent.querySelector('.loading-spinner');
            if (spinner) {
                this.viewContent.innerHTML = ''; // Clear it out
            }
        }

        renderError(message) {
            this.viewContent.innerHTML = `<div class="card" style="border-left: 5px solid var(--danger-color); padding: 1.5rem; color: var(--danger-color);">${message}</div>`;
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
                { key: 'status', label: 'Status' },
                { key: 'actions', label: 'Actions' }
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
                { key: 'lastLogin', label: 'Last Login', format: (d) => this.utils.formatDate(d, true) }
            ]);
            
            const logsTable = this.generateTable(logs.slice(0, 10), [ // show last 10 logs
                { key: 'timestamp', label: 'Timestamp', format: (ts) => new Date(ts).toLocaleString() },
                { key: 'user', label: 'User' },
                { key: 'action', label: 'Action' }
            ]);

            this.viewContent.innerHTML = `
                <div class="grid-container" style="grid-template-columns: 1fr;">
                    <div class="card">
                        <div class="card-header d-flex justify-between align-center">
                            <span>User Management</span>
                        </div>
                        <div class="table-container">${usersTable}</div>
                    </div>
                    <div class="card">
                        <div class="card-header">System Logs (Last 10)</div>
                        <div class="table-container">${logsTable}</div>
                    </div>
                </div>
            `;
        }
        
        generateTable(data, columns) {
            if (!data || data.length === 0) {
                return '<p>No data available.</p>';
            }
            
            const headerRow = `<tr>${columns.map(col => `<th>${col.label}</th>`).join('')}</tr>`;
            
            const rows = data.map(item => {
                const cells = columns.map(col => {
                    if (col.key === 'actions') {
                        return `
                            <td>
                                <button class="btn btn-sm btn-info btn-icon" data-doc-action="view" data-id="${item.id}" title="View">
                                    <i class="fas fa-eye"></i>
                                </button>
                                <button class="btn btn-sm btn-warning btn-icon" data-doc-action="edit" data-id="${item.id}" title="Edit">
                                    <i class="fas fa-pen"></i>
                                </button>
                                <button class="btn btn-sm btn-danger btn-icon" data-doc-action="delete" data-id="${item.id}" title="Delete">
                                    <i class="fas fa-trash-alt"></i>
                                </button>
                                <button class="btn btn-sm btn-success btn-icon" data-doc-action="print" data-id="${item.id}" title="Print">
                                    <i class="fas fa-print"></i>
                                </button>
                            </td>
                        `;
                    }
                    const value = item[col.key];
                    const formattedValue = col.format ? col.format(value) : value;
                    return `<td>${formattedValue || '-'}</td>`;
                }).join('');
                return `<tr>${cells}</tr>`;
            }).join('');
            
            return `
                <table>
                    <thead>${headerRow}</thead>
                    <tbody>${rows}</tbody>
                </table>
            `;
        }
        
        showFormModal(type, doc = null) {
            const isEditing = !!doc;
            const title = isEditing ? `Edit ${this.utils.capitalize(type)}` : `Create New ${this.utils.capitalize(type)}`;
            
            // Generate form fields
            const formFields = `
                <input type="hidden" name="type" value="${type}">
                ${isEditing ? `<input type="hidden" name="id" value="${doc.id}">` : ''}
                <div class="grid-container" style="grid-template-columns: 1fr 1fr;">
                    <div class="form-group">
                        <label for="clientName">Client Name</label>
                        <input type="text" id="clientName" name="clientName" class="form-control" value="${doc?.clientName || ''}" required>
                    </div>
                    <div class="form-group">
                        <label for="date">Date</label>
                        <input type="date" id="date" name="date" class="form-control" value="${doc?.date || this.utils.formatDate(new Date())}" required>
                    </div>
                </div>
                <div class="d-flex justify-between mt-2">
                    <button type="submit" class="btn btn-primary"><i class="fas fa-save"></i> Save</button>
                </div>
            `;

            this.modalContainer.innerHTML = this.generateModalHtml(title, `
                <form id="document-form">
                    ${formFields}
                </form>
            `);
            this.modalContainer.classList.remove('hidden');
        }
        
        showDocumentDetail(doc, action) {
            const title = `${this.utils.capitalize(doc.type)} #${doc.number}`;
            const items = JSON.parse(doc.items || '[]').map(item => `
                <li>${item.description} - ${this.utils.formatCurrency(item.amount)}</li>
            `).join('');

            const content = `
                <div class="d-flex justify-between">
                    <div>
                        <strong>Client:</strong> ${doc.clientName}<br>
                        <strong>Date:</strong> ${this.utils.formatDate(doc.date)}
                    </div>
                    <div>
                        <strong>Total:</strong> ${this.utils.formatCurrency(doc.total)}<br>
                        <strong>Status:</strong> ${doc.status}
                    </div>
                </div>
                <hr style="margin: 1.5rem 0; border-color: var(--border-color);">
                <h4>Items</h4>
                <ul>${items || 'No items listed.'}</ul>
            `;
            
            this.modalContainer.innerHTML = this.generateModalHtml(title, content);
            this.modalContainer.classList.remove('hidden');
            
            if (action === 'print') {
                // Wait for the modal to render, then trigger print
                setTimeout(() => {
                    const printWindow = window.open('', '', 'height=600,width=800');
                    printWindow.document.write('<html><head><title>Print Document</title>');
                    printWindow.document.write('<style>body{font-family: Arial, sans-serif;}</style>');
                    printWindow.document.write('</head><body>');
                    printWindow.document.write(this.modalContainer.querySelector('.modal-content').innerHTML);
                    printWindow.document.write('</body></html>');
                    printWindow.document.close();
                    printWindow.focus();
                    printWindow.print();
                    this.hideModal();
                }, 500);
            }
        }
        
        showShareOptions(doc) {
            const shareUrl = `https://your-domain.com/view/${doc.id}`; // Example share URL
            const content = `
                <p>Share this document via a public link:</p>
                <div class="d-flex align-center gap-1">
                    <input type="text" class="form-control" value="${shareUrl}" readonly>
                    <button class="btn btn-primary" onclick="navigator.clipboard.writeText('${shareUrl}'); alert('Link copied!');">
                        <i class="fas fa-copy"></i>
                    </button>
                </div>
                <div class="mt-2">
                    <p>Or share via email:</p>
                    <a href="mailto:?subject=Check out this document&body=Here is the document: ${shareUrl}" class="btn btn-info" target="_blank">
                        <i class="fas fa-envelope"></i> Send Email
                    </a>
                </div>
            `;
            this.modalContainer.innerHTML = this.generateModalHtml('Share Document', content);
            this.modalContainer.classList.remove('hidden');
        }

        generateModalHtml(title, content) {
            return `
                <div class="modal-overlay">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h3>${title}</h3>
                            <button class="close-modal"><i class="fas fa-times"></i></button>
                        </div>
                        <div>
                            ${content}
                        </div>
                    </div>
                </div>
            `;
        }

        hideModal() {
            this.modalContainer.innerHTML = '';
            this.modalContainer.classList.add('hidden');
        }
        
        getFormItems() {
            // This is a placeholder. You'll need to implement the logic
            // to gather data from dynamic form fields for document items.
            return [
                { description: 'Sample Item 1', amount: 5000 },
                { description: 'Sample Item 2', amount: 7500 }
            ];
        }
    }


    /**
     * =================================================================
     * UTILITY CLASS
     * Contains helper functions for formatting and other tasks.
     * =================================================================
     */
    class Utils {
        formatCurrency(amount) {
            return new Intl.NumberFormat('en-IN', { style: 'currency', currency: 'INR' }).format(amount);
        }

        formatDate(dateString, includeTime = false) {
            const date = new Date(dateString);
            const options = {
                year: 'numeric',
                month: 'short',
                day: 'numeric'
            };
            if (includeTime) {
                options.hour = '2-digit';
                options.minute = '2-digit';
            }
            return date.toLocaleDateString('en-US', options);
        }

        capitalize(string) {
            if (!string) return '';
            return string.charAt(0).toUpperCase() + string.slice(1);
        }
    }

    // Initialize the app
    const app = new App();
    app.init();
    </script>
</body>
</html>
