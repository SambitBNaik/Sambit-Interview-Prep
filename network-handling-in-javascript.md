# Network Handling in JavaScript - Complete Guide

## Table of Contents
1. [Introduction to Network Handling](#introduction-to-network-handling)
2. [Fetch API](#fetch-api)
3. [XMLHttpRequest (Legacy)](#xmlhttprequest-legacy)
4. [HTTP Methods](#http-methods)
5. [Request Configuration](#request-configuration)
6. [Response Handling](#response-handling)
7. [Error Handling](#error-handling)
8. [Authentication](#authentication)
9. [File Uploads](#file-uploads)
10. [WebSockets](#websockets)
11. [Server-Sent Events (SSE)](#server-sent-events-sse)
12. [Interceptors and Middleware](#interceptors-and-middleware)
13. [Caching Strategies](#caching-strategies)
14. [Performance Optimization](#performance-optimization)
15. [Security Considerations](#security-considerations)
16. [Testing Network Code](#testing-network-code)
17. [Best Practices](#best-practices)
18. [Common Patterns](#common-patterns)

## Introduction to Network Handling

Network handling in JavaScript involves making HTTP requests to communicate with servers, APIs, and external services. Modern JavaScript provides several APIs and techniques for network communication:

- **Fetch API**: Modern, promise-based HTTP client
- **XMLHttpRequest**: Legacy HTTP request API
- **WebSockets**: Real-time bidirectional communication
- **Server-Sent Events**: Server-to-client streaming
- **Service Workers**: Background network handling

## Fetch API

The Fetch API is the modern standard for making HTTP requests in JavaScript. It provides a more powerful and flexible feature set than XMLHttpRequest.

### Basic GET Request
```javascript
// Simple GET request
fetch('https://api.example.com/data')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error('Error:', error));

// Using async/await
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error:', error);
    }
}
```

### Basic POST Request
```javascript
// POST request with JSON data
async function postData() {
    try {
        const response = await fetch('https://api.example.com/users', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                name: 'John Doe',
                email: 'john@example.com'
            })
        });
        
        const data = await response.json();
        console.log('Success:', data);
    } catch (error) {
        console.error('Error:', error);
    }
}
```

### Fetch Options
```javascript
const options = {
    method: 'GET',              // HTTP method
    headers: {                  // Request headers
        'Content-Type': 'application/json',
        'Authorization': 'Bearer token123'
    },
    body: JSON.stringify(data), // Request body
    mode: 'cors',              // cors, no-cors, same-origin
    cache: 'no-cache',         // default, no-cache, reload, force-cache
    credentials: 'same-origin', // include, same-origin, omit
    redirect: 'follow',        // manual, follow, error
    referrerPolicy: 'no-referrer', // Referrer policy
    signal: abortController.signal // AbortController signal
};

const response = await fetch(url, options);
```

## XMLHttpRequest (Legacy)

XMLHttpRequest is the older API for making HTTP requests. While Fetch is preferred, XMLHttpRequest is still used in some scenarios.

### Basic XMLHttpRequest
```javascript
function makeRequest(method, url, data = null) {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        
        xhr.open(method, url);
        xhr.setRequestHeader('Content-Type', 'application/json');
        
        xhr.onload = function() {
            if (xhr.status >= 200 && xhr.status < 300) {
                resolve(JSON.parse(xhr.responseText));
            } else {
                reject(new Error(`HTTP ${xhr.status}: ${xhr.statusText}`));
            }
        };
        
        xhr.onerror = function() {
            reject(new Error('Network error'));
        };
        
        xhr.send(data ? JSON.stringify(data) : null);
    });
}

// Usage
makeRequest('GET', 'https://api.example.com/data')
    .then(data => console.log(data))
    .catch(error => console.error(error));
```

### XMLHttpRequest with Progress
```javascript
function uploadWithProgress(url, file, onProgress) {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        const formData = new FormData();
        formData.append('file', file);
        
        xhr.upload.addEventListener('progress', (event) => {
            if (event.lengthComputable) {
                const percentComplete = (event.loaded / event.total) * 100;
                onProgress(percentComplete);
            }
        });
        
        xhr.addEventListener('load', () => {
            if (xhr.status >= 200 && xhr.status < 300) {
                resolve(JSON.parse(xhr.responseText));
            } else {
                reject(new Error(`Upload failed: ${xhr.statusText}`));
            }
        });
        
        xhr.addEventListener('error', () => {
            reject(new Error('Upload failed'));
        });
        
        xhr.open('POST', url);
        xhr.send(formData);
    });
}
```

## HTTP Methods

### GET Requests
```javascript
// Simple GET
const response = await fetch('/api/users');
const users = await response.json();

// GET with query parameters
const params = new URLSearchParams({
    page: 1,
    limit: 10,
    search: 'john'
});
const response = await fetch(`/api/users?${params}`);
```

### POST Requests
```javascript
// POST with JSON
const response = await fetch('/api/users', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
    },
    body: JSON.stringify({
        name: 'John Doe',
        email: 'john@example.com'
    })
});

// POST with FormData
const formData = new FormData();
formData.append('name', 'John Doe');
formData.append('avatar', fileInput.files[0]);

const response = await fetch('/api/users', {
    method: 'POST',
    body: formData
});
```

### PUT Requests
```javascript
// Update entire resource
const response = await fetch('/api/users/123', {
    method: 'PUT',
    headers: {
        'Content-Type': 'application/json',
    },
    body: JSON.stringify({
        id: 123,
        name: 'John Smith',
        email: 'johnsmith@example.com'
    })
});
```

### PATCH Requests
```javascript
// Partial update
const response = await fetch('/api/users/123', {
    method: 'PATCH',
    headers: {
        'Content-Type': 'application/json',
    },
    body: JSON.stringify({
        name: 'John Smith' // Only update name
    })
});
```

### DELETE Requests
```javascript
// Delete resource
const response = await fetch('/api/users/123', {
    method: 'DELETE'
});

if (response.ok) {
    console.log('User deleted successfully');
}
```

## Request Configuration

### Headers
```javascript
const headers = new Headers();
headers.append('Content-Type', 'application/json');
headers.append('Authorization', 'Bearer ' + token);
headers.append('X-Custom-Header', 'custom-value');

// Or use object literal
const headers = {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${token}`,
    'Accept': 'application/json'
};

const response = await fetch('/api/data', {
    method: 'GET',
    headers: headers
});
```

### Query Parameters
```javascript
// Using URLSearchParams
function buildUrl(baseUrl, params) {
    const url = new URL(baseUrl);
    Object.keys(params).forEach(key => {
        if (params[key] !== null && params[key] !== undefined) {
            url.searchParams.append(key, params[key]);
        }
    });
    return url.toString();
}

const url = buildUrl('/api/users', {
    page: 1,
    limit: 10,
    search: 'john',
    active: true
});
// Result: /api/users?page=1&limit=10&search=john&active=true
```

### Request Body
```javascript
// JSON body
const jsonBody = JSON.stringify({
    name: 'John',
    age: 30
});

// FormData body
const formData = new FormData();
formData.append('name', 'John');
formData.append('file', fileInput.files[0]);

// URLSearchParams body (form-encoded)
const formBody = new URLSearchParams();
formBody.append('name', 'John');
formBody.append('email', 'john@example.com');

// Blob body
const blob = new Blob(['Hello World'], { type: 'text/plain' });
```

## Response Handling

### Response Properties
```javascript
const response = await fetch('/api/data');

console.log(response.status);        // 200
console.log(response.statusText);    // "OK"
console.log(response.ok);           // true for 200-299
console.log(response.headers);      // Headers object
console.log(response.url);          // Final URL after redirects
console.log(response.type);         // "basic", "cors", "error"
console.log(response.redirected);   // Boolean
```

### Reading Response Body
```javascript
const response = await fetch('/api/data');

// JSON
const jsonData = await response.json();

// Text
const textData = await response.text();

// Blob
const blobData = await response.blob();

// ArrayBuffer
const bufferData = await response.arrayBuffer();

// FormData
const formData = await response.formData();

// Stream (for large responses)
const reader = response.body.getReader();
```

### Streaming Response
```javascript
async function streamResponse() {
    const response = await fetch('/api/large-data');
    const reader = response.body.getReader();
    
    while (true) {
        const { done, value } = await reader.read();
        
        if (done) break;
        
        // Process chunk
        console.log('Received chunk:', value);
    }
}
```

### Response Headers
```javascript
const response = await fetch('/api/data');

// Get specific header
const contentType = response.headers.get('content-type');
const authHeader = response.headers.get('authorization');

// Check if header exists
if (response.headers.has('x-rate-limit')) {
    const rateLimit = response.headers.get('x-rate-limit');
}

// Iterate headers
for (const [key, value] of response.headers) {
    console.log(`${key}: ${value}`);
}
```

## Error Handling

### Basic Error Handling
```javascript
async function fetchWithErrorHandling(url) {
    try {
        const response = await fetch(url);
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        return await response.json();
    } catch (error) {
        console.error('Fetch error:', error);
        throw error;
    }
}
```

### Comprehensive Error Handling
```javascript
class NetworkError extends Error {
    constructor(message, status, response) {
        super(message);
        this.name = 'NetworkError';
        this.status = status;
        this.response = response;
    }
}

async function robustFetch(url, options = {}) {
    try {
        const response = await fetch(url, options);
        
        if (!response.ok) {
            let errorMessage = `HTTP ${response.status}: ${response.statusText}`;
            
            // Try to get error details from response
            try {
                const errorData = await response.json();
                errorMessage = errorData.message || errorMessage;
            } catch (e) {
                // Response is not JSON, use default message
            }
            
            throw new NetworkError(errorMessage, response.status, response);
        }
        
        return response;
    } catch (error) {
        if (error instanceof TypeError) {
            // Network error (no internet, CORS, etc.)
            throw new NetworkError('Network error: Unable to connect', 0, null);
        }
        throw error;
    }
}

// Usage
try {
    const response = await robustFetch('/api/data');
    const data = await response.json();
} catch (error) {
    if (error instanceof NetworkError) {
        console.error(`Network error ${error.status}: ${error.message}`);
    } else {
        console.error('Unexpected error:', error);
    }
}
```

### Retry Logic
```javascript
async function fetchWithRetry(url, options = {}, maxRetries = 3, delay = 1000) {
    let lastError;
    
    for (let attempt = 1; attempt <= maxRetries; attempt++) {
        try {
            const response = await fetch(url, options);
            
            if (response.ok) {
                return response;
            }
            
            // Don't retry client errors (4xx)
            if (response.status >= 400 && response.status < 500) {
                throw new Error(`HTTP ${response.status}: ${response.statusText}`);
            }
            
            lastError = new Error(`HTTP ${response.status}: ${response.statusText}`);
        } catch (error) {
            lastError = error;
        }
        
        if (attempt < maxRetries) {
            console.log(`Attempt ${attempt} failed, retrying in ${delay}ms...`);
            await new Promise(resolve => setTimeout(resolve, delay));
            delay *= 2; // Exponential backoff
        }
    }
    
    throw lastError;
}
```

## Authentication

### Bearer Token Authentication
```javascript
class ApiClient {
    constructor(baseUrl, token = null) {
        this.baseUrl = baseUrl;
        this.token = token;
    }
    
    setToken(token) {
        this.token = token;
    }
    
    async request(endpoint, options = {}) {
        const url = `${this.baseUrl}${endpoint}`;
        const config = {
            ...options,
            headers: {
                'Content-Type': 'application/json',
                ...options.headers
            }
        };
        
        if (this.token) {
            config.headers.Authorization = `Bearer ${this.token}`;
        }
        
        const response = await fetch(url, config);
        
        if (response.status === 401) {
            // Token expired or invalid
            this.token = null;
            throw new Error('Authentication required');
        }
        
        if (!response.ok) {
            throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }
        
        return response;
    }
    
    async get(endpoint) {
        const response = await this.request(endpoint);
        return response.json();
    }
    
    async post(endpoint, data) {
        const response = await this.request(endpoint, {
            method: 'POST',
            body: JSON.stringify(data)
        });
        return response.json();
    }
}

// Usage
const api = new ApiClient('https://api.example.com');
api.setToken('your-jwt-token');

try {
    const users = await api.get('/users');
    const newUser = await api.post('/users', { name: 'John', email: 'john@example.com' });
} catch (error) {
    if (error.message === 'Authentication required') {
        // Redirect to login or refresh token
    }
}
```

### Basic Authentication
```javascript
function makeAuthenticatedRequest(url, username, password) {
    const credentials = btoa(`${username}:${password}`);
    
    return fetch(url, {
        headers: {
            'Authorization': `Basic ${credentials}`
        }
    });
}
```

### OAuth 2.0 Flow
```javascript
class OAuthClient {
    constructor(clientId, redirectUri) {
        this.clientId = clientId;
        this.redirectUri = redirectUri;
        this.accessToken = localStorage.getItem('accessToken');
        this.refreshToken = localStorage.getItem('refreshToken');
    }
    
    // Redirect to OAuth provider
    authorize(scope = 'read') {
        const params = new URLSearchParams({
            response_type: 'code',
            client_id: this.clientId,
            redirect_uri: this.redirectUri,
            scope: scope,
            state: this.generateState()
        });
        
        localStorage.setItem('oauth_state', params.get('state'));
        window.location.href = `https://auth.example.com/oauth/authorize?${params}`;
    }
    
    // Handle callback from OAuth provider
    async handleCallback() {
        const urlParams = new URLSearchParams(window.location.search);
        const code = urlParams.get('code');
        const state = urlParams.get('state');
        
        if (state !== localStorage.getItem('oauth_state')) {
            throw new Error('Invalid state parameter');
        }
        
        const response = await fetch('https://auth.example.com/oauth/token', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                grant_type: 'authorization_code',
                code: code,
                client_id: this.clientId,
                redirect_uri: this.redirectUri
            })
        });
        
        const tokens = await response.json();
        this.accessToken = tokens.access_token;
        this.refreshToken = tokens.refresh_token;
        
        localStorage.setItem('accessToken', this.accessToken);
        localStorage.setItem('refreshToken', this.refreshToken);
    }
    
    async refreshAccessToken() {
        if (!this.refreshToken) {
            throw new Error('No refresh token available');
        }
        
        const response = await fetch('https://auth.example.com/oauth/token', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                grant_type: 'refresh_token',
                refresh_token: this.refreshToken,
                client_id: this.clientId
            })
        });
        
        const tokens = await response.json();
        this.accessToken = tokens.access_token;
        localStorage.setItem('accessToken', this.accessToken);
    }
    
    generateState() {
        return Math.random().toString(36).substring(2, 15) + 
               Math.random().toString(36).substring(2, 15);
    }
}
```

## File Uploads

### Simple File Upload
```javascript
async function uploadFile(file, url) {
    const formData = new FormData();
    formData.append('file', file);
    
    const response = await fetch(url, {
        method: 'POST',
        body: formData
    });
    
    if (!response.ok) {
        throw new Error(`Upload failed: ${response.statusText}`);
    }
    
    return response.json();
}

// Usage
const fileInput = document.getElementById('fileInput');
fileInput.addEventListener('change', async (event) => {
    const file = event.target.files[0];
    if (file) {
        try {
            const result = await uploadFile(file, '/api/upload');
            console.log('Upload successful:', result);
        } catch (error) {
            console.error('Upload failed:', error);
        }
    }
});
```

### Upload with Progress
```javascript
function uploadWithProgress(file, url, onProgress) {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        const formData = new FormData();
        formData.append('file', file);
        
        xhr.upload.addEventListener('progress', (event) => {
            if (event.lengthComputable) {
                const percentComplete = (event.loaded / event.total) * 100;
                onProgress(percentComplete);
            }
        });
        
        xhr.addEventListener('load', () => {
            if (xhr.status >= 200 && xhr.status < 300) {
                try {
                    resolve(JSON.parse(xhr.responseText));
                } catch (e) {
                    resolve(xhr.responseText);
                }
            } else {
                reject(new Error(`Upload failed: ${xhr.statusText}`));
            }
        });
        
        xhr.addEventListener('error', () => {
            reject(new Error('Upload failed'));
        });
        
        xhr.open('POST', url);
        xhr.send(formData);
    });
}

// Usage
const progressBar = document.getElementById('progressBar');
uploadWithProgress(file, '/api/upload', (percent) => {
    progressBar.value = percent;
    console.log(`Upload progress: ${percent}%`);
});
```

### Multiple File Upload
```javascript
async function uploadMultipleFiles(files, url) {
    const formData = new FormData();
    
    for (let i = 0; i < files.length; i++) {
        formData.append('files', files[i]);
    }
    
    const response = await fetch(url, {
        method: 'POST',
        body: formData
    });
    
    return response.json();
}

// Chunked upload for large files
async function uploadLargeFile(file, url, chunkSize = 1024 * 1024) {
    const totalChunks = Math.ceil(file.size / chunkSize);
    const fileName = file.name;
    const fileId = Date.now().toString();
    
    for (let chunkIndex = 0; chunkIndex < totalChunks; chunkIndex++) {
        const start = chunkIndex * chunkSize;
        const end = Math.min(start + chunkSize, file.size);
        const chunk = file.slice(start, end);
        
        const formData = new FormData();
        formData.append('chunk', chunk);
        formData.append('chunkIndex', chunkIndex);
        formData.append('totalChunks', totalChunks);
        formData.append('fileName', fileName);
        formData.append('fileId', fileId);
        
        const response = await fetch(url, {
            method: 'POST',
            body: formData
        });
        
        if (!response.ok) {
            throw new Error(`Chunk ${chunkIndex} upload failed`);
        }
        
        console.log(`Uploaded chunk ${chunkIndex + 1}/${totalChunks}`);
    }
    
    return fileId;
}
```

## WebSockets

### Basic WebSocket Connection
```javascript
class WebSocketClient {
    constructor(url) {
        this.url = url;
        this.ws = null;
        this.listeners = {};
        this.reconnectAttempts = 0;
        this.maxReconnectAttempts = 5;
        this.reconnectDelay = 1000;
    }
    
    connect() {
        this.ws = new WebSocket(this.url);
        
        this.ws.onopen = (event) => {
            console.log('WebSocket connected');
            this.reconnectAttempts = 0;
            this.emit('open', event);
        };
        
        this.ws.onmessage = (event) => {
            try {
                const data = JSON.parse(event.data);
                this.emit('message', data);
                
                // Handle specific message types
                if (data.type) {
                    this.emit(data.type, data);
                }
            } catch (error) {
                this.emit('message', event.data);
            }
        };
        
        this.ws.onclose = (event) => {
            console.log('WebSocket closed', event.code, event.reason);
            this.emit('close', event);
            
            if (!event.wasClean && this.reconnectAttempts < this.maxReconnectAttempts) {
                this.reconnect();
            }
        };
        
        this.ws.onerror = (error) => {
            console.error('WebSocket error:', error);
            this.emit('error', error);
        };
    }
    
    reconnect() {
        this.reconnectAttempts++;
        const delay = this.reconnectDelay * Math.pow(2, this.reconnectAttempts - 1);
        
        console.log(`Reconnecting in ${delay}ms... (attempt ${this.reconnectAttempts})`);
        
        setTimeout(() => {
            this.connect();
        }, delay);
    }
    
    send(data) {
        if (this.ws && this.ws.readyState === WebSocket.OPEN) {
            this.ws.send(JSON.stringify(data));
        } else {
            console.error('WebSocket is not connected');
        }
    }
    
    close() {
        if (this.ws) {
            this.ws.close();
        }
    }
    
    on(event, callback) {
        if (!this.listeners[event]) {
            this.listeners[event] = [];
        }
        this.listeners[event].push(callback);
    }
    
    off(event, callback) {
        if (this.listeners[event]) {
            this.listeners[event] = this.listeners[event].filter(cb => cb !== callback);
        }
    }
    
    emit(event, data) {
        if (this.listeners[event]) {
            this.listeners[event].forEach(callback => callback(data));
        }
    }
}

// Usage
const wsClient = new WebSocketClient('wss://api.example.com/ws');

wsClient.on('open', () => {
    console.log('Connected to WebSocket');
    wsClient.send({ type: 'subscribe', channel: 'notifications' });
});

wsClient.on('message', (data) => {
    console.log('Received:', data);
});

wsClient.on('notification', (data) => {
    showNotification(data.message);
});

wsClient.connect();
```

### WebSocket with Authentication
```javascript
class AuthenticatedWebSocket extends WebSocketClient {
    constructor(url, getToken) {
        super(url);
        this.getToken = getToken;
    }
    
    connect() {
        const token = this.getToken();
        const authenticatedUrl = `${this.url}?token=${token}`;
        
        this.ws = new WebSocket(authenticatedUrl);
        // ... rest of connection logic
    }
    
    async refreshTokenAndReconnect() {
        try {
            await this.refreshToken();
            this.connect();
        } catch (error) {
            console.error('Failed to refresh token:', error);
        }
    }
}
```

## Server-Sent Events (SSE)

### Basic SSE Connection
```javascript
class EventSourceClient {
    constructor(url) {
        this.url = url;
        this.eventSource = null;
        this.listeners = {};
    }
    
    connect() {
        this.eventSource = new EventSource(this.url);
        
        this.eventSource.onopen = (event) => {
            console.log('SSE connection opened');
            this.emit('open', event);
        };
        
        this.eventSource.onmessage = (event) => {
            try {
                const data = JSON.parse(event.data);
                this.emit('message', data);
            } catch (error) {
                this.emit('message', event.data);
            }
        };
        
        this.eventSource.onerror = (error) => {
            console.error('SSE error:', error);
            this.emit('error', error);
        };
        
        // Custom event types
        this.eventSource.addEventListener('notification', (event) => {
            const data = JSON.parse(event.data);
            this.emit('notification', data);
        });
    }
    
    close() {
        if (this.eventSource) {
            this.eventSource.close();
        }
    }
    
    on(event, callback) {
        if (!this.listeners[event]) {
            this.listeners[event] = [];
        }
        this.listeners[event].push(callback);
    }
    
    emit(event, data) {
        if (this.listeners[event]) {
            this.listeners[event].forEach(callback => callback(data));
        }
    }
}

// Usage
const sseClient = new EventSourceClient('/api/events');

sseClient.on('message', (data) => {
    console.log('Received message:', data);
});

sseClient.on('notification', (data) => {
    showNotification(data.title, data.message);
});

sseClient.connect();
```

## Interceptors and Middleware

### Request/Response Interceptors
```javascript
class ApiClient {
    constructor(baseUrl) {
        this.baseUrl = baseUrl;
        this.requestInterceptors = [];
        this.responseInterceptors = [];
    }
    
    addRequestInterceptor(interceptor) {
        this.requestInterceptors.push(interceptor);
    }
    
    addResponseInterceptor(interceptor) {
        this.responseInterceptors.push(interceptor);
    }
    
    async request(url, options = {}) {
        // Apply request interceptors
        let finalOptions = { ...options };
        for (const interceptor of this.requestInterceptors) {
            finalOptions = await interceptor(url, finalOptions);
        }
        
        // Make request
        let response = await fetch(`${this.baseUrl}${url}`, finalOptions);
        
        // Apply response interceptors
        for (const interceptor of this.responseInterceptors) {
            response = await interceptor(response);
        }
        
        return response;
    }
}

// Usage
const api = new ApiClient('https://api.example.com');

// Add authentication interceptor
api.addRequestInterceptor(async (url, options) => {
    const token = localStorage.getItem('authToken');
    if (token) {
        options.headers = {
            ...options.headers,
            'Authorization': `Bearer ${token}`
        };
    }
    return options;
});

// Add logging interceptor
api.addResponseInterceptor(async (response) => {
    console.log(`${response.status} ${response.url}`);
    return response;
});

// Add error handling interceptor
api.addResponseInterceptor(async (response) => {
    if (response.status === 401) {
        // Redirect to login
        window.location.href = '/login';
    }
    return response;
});
```

## Caching Strategies

### In-Memory Cache
```javascript
class CacheManager {
    constructor(defaultTtl = 5 * 60 * 1000) { // 5 minutes
        this.cache = new Map();
        this.defaultTtl = defaultTtl;
    }
    
    set(key, value, ttl = this.defaultTtl) {
        const expiresAt = Date.now() + ttl;
        this.cache.set(key, { value, expiresAt });
    }
    
    get(key) {
        const cached = this.cache.get(key);
        
        if (!cached) {
            return null;
        }
        
        if (Date.now() > cached.expiresAt) {
            this.cache.delete(key);
            return null;
        }
        
        return cached.value;
    }
    
    delete(key) {
        this.cache.delete(key);
    }
    
    clear() {
        this.cache.clear();
    }
    
    cleanup() {
        const now = Date.now();
        for (const [key, cached] of this.cache.entries()) {
            if (now > cached.expiresAt) {
                this.cache.delete(key);
            }
        }
    }
}

// HTTP Client with caching
class CachedApiClient {
    constructor(baseUrl) {
        this.baseUrl = baseUrl;
        this.cache = new CacheManager();
    }
    
    async get(endpoint, options = {}) {
        const cacheKey = `${endpoint}${JSON.stringify(options.params || {})}`;
        
        // Check cache first
        if (!options.bypassCache) {
            const cached = this.cache.get(cacheKey);
            if (cached) {
                console.log('Cache hit for:', cacheKey);
                return cached;
            }
        }
        
        // Make request
        const url = this.buildUrl(endpoint, options.params);
        const response = await fetch(url);
        const data = await response.json();
        
        // Cache response
        if (response.ok) {
            this.cache.set(cacheKey, data, options.cacheTtl);
        }
        
        return data;
    }
    
    buildUrl(endpoint, params = {}) {
        const url = new URL(`${this.baseUrl}${endpoint}`);
        Object.keys(params).forEach(key => {
            url.searchParams.append(key, params[key]);
        });
        return url.toString();
    }
}
```

### Browser Storage Cache
```javascript
class PersistentCache {
    constructor(storage = localStorage, prefix = 'cache_') {
        this.storage = storage;
        this.prefix = prefix;
    }
    
    set(key, value, ttl = 0) {
        const item = {
            value,
            expiresAt: ttl > 0 ? Date.now() + ttl : 0
        };
        
        try {
            this.storage.setItem(this.prefix + key, JSON.stringify(item));
        } catch (error) {
            console.error('Cache storage error:', error);
        }
    }
    
    get(key) {
        try {
            const item = this.storage.getItem(this.prefix + key);
            if (!item) return null;
            
            const parsed = JSON.parse(item);
            
            if (parsed.expiresAt > 0 && Date.now() > parsed.expiresAt) {
                this.delete(key);
                return null;
            }
            
            return parsed.value;
        } catch (error) {
            console.error('Cache retrieval error:', error);
            return null;
        }
    }
    
    delete(key) {
        this.storage.removeItem(this.prefix + key);
    }
    
    clear() {
        const keys = [];
        for (let i = 0; i < this.storage.length; i++) {
            const key = this.storage.key(i);
            if (key && key.startsWith(this.prefix)) {
                keys.push(key);
            }
        }
        keys.forEach(key => this.storage.removeItem(key));
    }
}
```

## Performance Optimization

### Request Deduplication
```javascript
class RequestDeduplicator {
    constructor() {
        this.activeRequests = new Map();
    }
    
    async request(key, requestFn) {
        // If request is already in progress, return the existing promise
        if (this.activeRequests.has(key)) {
            console.log('Deduplicating request:', key);
            return this.activeRequests.get(key);
        }
        
        // Start new request
        const promise = requestFn()
            .finally(() => {
                // Clean up when request completes
                this.activeRequests.delete(key);
            });
        
        this.activeRequests.set(key, promise);
        return promise;
    }
}

// Usage
const deduplicator = new RequestDeduplicator();

async function fetchUser(userId) {
    return deduplicator.request(`user-${userId}`, async () => {
        const response = await fetch(`/api/users/${userId}`);
        return response.json();
    });
}
```

### Request Batching
```javascript
class RequestBatcher {
    constructor(batchFn, delay = 50, maxBatchSize = 10) {
        this.batchFn = batchFn;
        this.delay = delay;
        this.maxBatchSize = maxBatchSize;
        this.queue = [];
        this.timer = null;
    }
    
    add(request) {
        return new Promise((resolve, reject) => {
            this.queue.push({ request, resolve, reject });
            
            if (this.queue.length >= this.maxBatchSize) {
                this.flush();
            } else if (!this.timer) {
                this.timer = setTimeout(() => this.flush(), this.delay);
            }
        });
    }
    
    async flush() {
        if (this.queue.length === 0) return;
        
        const batch = this.queue.splice(0);
        this.timer = null;
        
        try {
            const requests = batch.map(item => item.request);
            const results = await this.batchFn(requests);
            
            batch.forEach((item, index) => {
                item.resolve(results[index]);
            });
        } catch (error) {
            batch.forEach(item => {
                item.reject(error);
            });
        }
    }
}

// Usage for batching user lookups
const userBatcher = new RequestBatcher(async (userIds) => {
    const response = await fetch('/api/users', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ ids: userIds })
    });
    return response.json();
});

async function getUser(userId) {
    return userBatcher.add(userId);
}
```

### Connection Pooling (Conceptual)
```javascript
class ConnectionPool {
    constructor(maxConnections = 6) {
        this.maxConnections = maxConnections;
        this.activeConnections = 0;
        this.queue = [];
    }
    
    async request(url, options) {
        return new Promise((resolve, reject) => {
            const execute = async () => {
                this.activeConnections++;
                
                try {
                    const response = await fetch(url, options);
                    resolve(response);
                } catch (error) {
                    reject(error);
                } finally {
                    this.activeConnections--;
                    this.processQueue();
                }
            };
            
            if (this.activeConnections < this.maxConnections) {
                execute();
            } else {
                this.queue.push({ execute, resolve, reject });
            }
        });
    }
    
    processQueue() {
        if (this.queue.length > 0 && this.activeConnections < this.maxConnections) {
            const { execute } = this.queue.shift();
            execute();
        }
    }
}
```

## Security Considerations

### CSRF Protection
```javascript
class CSRFProtectedClient {
    constructor(baseUrl) {
        this.baseUrl = baseUrl;
        this.csrfToken = null;
    }
    
    async getCSRFToken() {
        if (!this.csrfToken) {
            const response = await fetch('/api/csrf-token');
            const data = await response.json();
            this.csrfToken = data.token;
        }
        return this.csrfToken;
    }
    
    async request(endpoint, options = {}) {
        const token = await this.getCSRFToken();
        
        const config = {
            ...options,
            headers: {
                'X-CSRF-Token': token,
                ...options.headers
            }
        };
        
        return fetch(`${this.baseUrl}${endpoint}`, config);
    }
}
```

### Input Sanitization
```javascript
function sanitizeInput(input) {
    if (typeof input === 'string') {
        return input
            .replace(/[<>]/g, '') // Remove potentially dangerous characters
            .trim()
            .slice(0, 1000); // Limit length
    }
    return input;
}

function sanitizeObject(obj) {
    const sanitized = {};
    for (const [key, value] of Object.entries(obj)) {
        if (typeof value === 'object' && value !== null) {
            sanitized[key] = sanitizeObject(value);
        } else {
            sanitized[key] = sanitizeInput(value);
        }
    }
    return sanitized;
}
```

### Content Security Policy Headers
```javascript
async function secureRequest(url, options = {}) {
    const secureOptions = {
        ...options,
        headers: {
            'X-Content-Type-Options': 'nosniff',
            'X-Frame-Options': 'DENY',
            'X-XSS-Protection': '1; mode=block',
            ...options.headers
        }
    };
    
    return fetch(url, secureOptions);
}
```

## Testing Network Code

### Mocking Fetch
```javascript
// Mock implementation for testing
class FetchMock {
    constructor() {
        this.mocks = new Map();
        this.calls = [];
    }
    
    mock(url, response) {
        this.mocks.set(url, response);
    }
    
    mockOnce(url, response) {
        this.mocks.set(url, { ...response, once: true });
    }
    
    async fetch(url, options = {}) {
        this.calls.push({ url, options });
        
        const mock = this.mocks.get(url);
        if (!mock) {
            throw new Error(`No mock found for ${url}`);
        }
        
        if (mock.once) {
            this.mocks.delete(url);
        }
        
        if (mock.error) {
            throw new Error(mock.error);
        }
        
        return new Response(JSON.stringify(mock.data), {
            status: mock.status || 200,
            headers: mock.headers || {}
        });
    }
    
    getCalls() {
        return this.calls;
    }
    
    clear() {
        this.mocks.clear();
        this.calls = [];
    }
}

// Usage in tests
const fetchMock = new FetchMock();
global.fetch = fetchMock.fetch.bind(fetchMock);

// Test setup
fetchMock.mock('/api/users', {
    data: [{ id: 1, name: 'John' }],
    status: 200
});

// Test
const users = await fetchUsers();
expect(users).toEqual([{ id: 1, name: 'John' }]);
expect(fetchMock.getCalls()).toHaveLength(1);
```

### Network Error Testing
```javascript
async function testNetworkErrors() {
    const scenarios = [
        { error: 'Network timeout', delay: 5000 },
        { status: 404, message: 'Not found' },
        { status: 500, message: 'Server error' },
        { status: 401, message: 'Unauthorized' }
    ];
    
    for (const scenario of scenarios) {
        try {
            fetchMock.mockOnce('/api/test', scenario);
            await apiCall('/api/test');
        } catch (error) {
            console.log(`Handled error: ${error.message}`);
        }
    }
}
```

## Best Practices

### 1. Always Handle Errors
```javascript
// Good
async function fetchData(url) {
    try {
        const response = await fetch(url);
        
        if (!response.ok) {
            throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }
        
        return await response.json();
    } catch (error) {
        console.error('Fetch error:', error);
        // Handle error appropriately
        throw error;
    }
}
```

### 2. Use Appropriate HTTP Methods
```javascript
// RESTful API calls
const api = {
    // GET - Retrieve data
    getUsers: () => fetch('/api/users'),
    
    // POST - Create new resource
    createUser: (userData) => fetch('/api/users', {
        method: 'POST',
        body: JSON.stringify(userData)
    }),
    
    // PUT - Replace entire resource
    updateUser: (id, userData) => fetch(`/api/users/${id}`, {
        method: 'PUT',
        body: JSON.stringify(userData)
    }),
    
    // PATCH - Partial update
    patchUser: (id, changes) => fetch(`/api/users/${id}`, {
        method: 'PATCH',
        body: JSON.stringify(changes)
    }),
    
    // DELETE - Remove resource
    deleteUser: (id) => fetch(`/api/users/${id}`, {
        method: 'DELETE'
    })
};
```

### 3. Implement Request Cancellation
```javascript
class CancellableRequest {
    constructor() {
        this.activeRequests = new Map();
    }
    
    async request(key, url, options = {}) {
        // Cancel previous request with same key
        this.cancel(key);
        
        const controller = new AbortController();
        this.activeRequests.set(key, controller);
        
        try {
            const response = await fetch(url, {
                ...options,
                signal: controller.signal
            });
            
            this.activeRequests.delete(key);
            return response;
        } catch (error) {
            if (error.name === 'AbortError') {
                console.log('Request cancelled:', key);
            }
            throw error;
        }
    }
    
    cancel(key) {
        const controller = this.activeRequests.get(key);
        if (controller) {
            controller.abort();
            this.activeRequests.delete(key);
        }
    }
    
    cancelAll() {
        for (const [key, controller] of this.activeRequests) {
            controller.abort();
        }
        this.activeRequests.clear();
    }
}
```

### 4. Use Proper Content-Type Headers
```javascript
const contentTypes = {
    json: 'application/json',
    form: 'application/x-www-form-urlencoded',
    multipart: 'multipart/form-data',
    text: 'text/plain',
    xml: 'application/xml'
};

function createRequest(data, type = 'json') {
    const options = {
        method: 'POST',
        headers: {}
    };
    
    switch (type) {
        case 'json':
            options.headers['Content-Type'] = contentTypes.json;
            options.body = JSON.stringify(data);
            break;
            
        case 'form':
            options.headers['Content-Type'] = contentTypes.form;
            options.body = new URLSearchParams(data).toString();
            break;
            
        case 'multipart':
            // Don't set Content-Type, let browser set it with boundary
            const formData = new FormData();
            Object.keys(data).forEach(key => {
                formData.append(key, data[key]);
            });
            options.body = formData;
            break;
    }
    
    return options;
}
```

### 5. Implement Rate Limiting
```javascript
class RateLimiter {
    constructor(maxRequests = 10, timeWindow = 60000) {
        this.maxRequests = maxRequests;
        this.timeWindow = timeWindow;
        this.requests = [];
    }
    
    canMakeRequest() {
        const now = Date.now();
        
        // Remove old requests outside time window
        this.requests = this.requests.filter(
            timestamp => now - timestamp < this.timeWindow
        );
        
        return this.requests.length < this.maxRequests;
    }
    
    async makeRequest(requestFn) {
        if (!this.canMakeRequest()) {
            const waitTime = this.timeWindow - (Date.now() - this.requests[0]);
            throw new Error(`Rate limit exceeded. Try again in ${waitTime}ms`);
        }
        
        this.requests.push(Date.now());
        return requestFn();
    }
}
```

## Common Patterns

### Repository Pattern
```javascript
class UserRepository {
    constructor(apiClient) {
        this.api = apiClient;
        this.cache = new Map();
    }
    
    async findById(id) {
        if (this.cache.has(id)) {
            return this.cache.get(id);
        }
        
        const user = await this.api.get(`/users/${id}`);
        this.cache.set(id, user);
        return user;
    }
    
    async findAll(filters = {}) {
        const params = new URLSearchParams(filters);
        return this.api.get(`/users?${params}`);
    }
    
    async create(userData) {
        const user = await this.api.post('/users', userData);
        this.cache.set(user.id, user);
        return user;
    }
    
    async update(id, userData) {
        const user = await this.api.put(`/users/${id}`, userData);
        this.cache.set(id, user);
        return user;
    }
    
    async delete(id) {
        await this.api.delete(`/users/${id}`);
        this.cache.delete(id);
    }
}
```

### Observer Pattern for Real-time Updates
```javascript
class RealtimeDataManager {
    constructor() {
        this.observers = [];
        this.data = new Map();
    }
    
    subscribe(observer) {
        this.observers.push(observer);
    }
    
    unsubscribe(observer) {
        const index = this.observers.indexOf(observer);
        if (index > -1) {
            this.observers.splice(index, 1);
        }
    }
    
    notify(event, data) {
        this.observers.forEach(observer => {
            if (observer[event]) {
                observer[event](data);
            }
        });
    }
    
    connectWebSocket(url) {
        const ws = new WebSocket(url);
        
        ws.onmessage = (event) => {
            const message = JSON.parse(event.data);
            
            switch (message.type) {
                case 'update':
                    this.data.set(message.id, message.data);
                    this.notify('onUpdate', message.data);
                    break;
                    
                case 'delete':
                    this.data.delete(message.id);
                    this.notify('onDelete', message.id);
                    break;
                    
                case 'create':
                    this.data.set(message.data.id, message.data);
                    this.notify('onCreate', message.data);
                    break;
            }
        };
        
        return ws;
    }
}

// Usage
const dataManager = new RealtimeDataManager();

const userListObserver = {
    onUpdate: (user) => updateUserInList(user),
    onCreate: (user) => addUserToList(user),
    onDelete: (userId) => removeUserFromList(userId)
};

dataManager.subscribe(userListObserver);
dataManager.connectWebSocket('wss://api.example.com/ws');
```

### Circuit Breaker Pattern
```javascript
class CircuitBreaker {
    constructor(threshold = 5, timeout = 60000) {
        this.threshold = threshold;
        this.timeout = timeout;
        this.failureCount = 0;
        this.state = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
        this.nextAttempt = Date.now();
    }
    
    async execute(requestFn) {
        if (this.state === 'OPEN') {
            if (Date.now() < this.nextAttempt) {
                throw new Error('Circuit breaker is OPEN');
            }
            this.state = 'HALF_OPEN';
        }
        
        try {
            const result = await requestFn();
            this.onSuccess();
            return result;
        } catch (error) {
            this.onFailure();
            throw error;
        }
    }
    
    onSuccess() {
        this.failureCount = 0;
        this.state = 'CLOSED';
    }
    
    onFailure() {
        this.failureCount++;
        
        if (this.failureCount >= this.threshold) {
            this.state = 'OPEN';
            this.nextAttempt = Date.now() + this.timeout;
        }
    }
}

// Usage
const circuitBreaker = new CircuitBreaker(3, 30000);

async function makeApiCall() {
    return circuitBreaker.execute(async () => {
        const response = await fetch('/api/unstable-service');
        if (!response.ok) {
            throw new Error(`HTTP ${response.status}`);
        }
        return response.json();
    });
}
```

---

## Conclusion

Network handling in JavaScript has evolved significantly with modern APIs like Fetch, WebSockets, and Server-Sent Events. This comprehensive guide covers:

**Key Takeaways:**
- Always implement proper error handling and user feedback
- Use appropriate HTTP methods and status codes
- Implement caching and optimization strategies
- Consider security implications (CSRF, XSS, input sanitization)
- Test network code thoroughly with mocks and error scenarios
- Use modern patterns like interceptors, circuit breakers, and request deduplication

**Best Practices:**
- Use async/await for cleaner code
- Implement request cancellation for better UX
- Cache responses when appropriate
- Handle offline scenarios gracefully
- Monitor and optimize network performance
- Follow RESTful principles for API design

Remember that network requests are inherently unreliable, so always design your applications with failure scenarios in mind and provide appropriate fallbacks and user feedback.