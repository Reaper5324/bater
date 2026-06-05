# Bater Frontend

A modern, responsive Single Page Application (SPA) built with vanilla JavaScript (ES6+), HTML5, and CSS3. Bater provides a seamless marketplace experience for buyers and sellers with real-time notifications and dynamic content.

## Project Overview

Bater Frontend is a client-side application that communicates with the PHP backend API. It features:
- **Hash-based routing** for single-page navigation
- **Modular component architecture** for reusable UI elements
- **Service layer** for centralized API communication
- **Responsive design** for desktop and mobile devices
- **Role-based UI** with separate experiences for buyers, sellers, and admins
- **Real-time notifications** for user feedback

## Architecture

```
htdocs/
├── index.html              # Main SPA entry point
├── 404.html                # Error page
├── css/                    # Stylesheets
│   ├── styles.css          # Main styles
│   ├── components.css      # Component styles
│   ├── responsive.css      # Mobile/responsive styles
│   └── variables.css       # CSS custom properties
├── js/                     # Application code
│   ├── app.js              # Main router and initializer
│   ├── config.js           # Configuration and constants
│   ├── components/         # Reusable UI components
│   │   ├── navbar.js
│   │   ├── notifications.js
│   │   ├── cart.js
│   │   ├── products.js
│   │   ├── reviews.js
│   │   ├── orders.js
│   │   ├── messages.js
│   │   └── empty-states.js
│   ├── pages/              # Page modules (views)
│   │   ├── home.js
│   │   ├── dashboard.js
│   │   ├── categories.js
│   │   ├── wishlist.js
│   │   ├── auth/
│   │   │   ├── login.js
│   │   │   ├── register.js
│   │   │   └── profile.js
│   │   ├── products/
│   │   │   ├── list.js
│   │   │   ├── detail.js
│   │   │   ├── create.js
│   │   │   └── seller-products.js
│   │   ├── shopping/
│   │   │   ├── cart.js
│   │   │   └── checkout.js
│   │   ├── orders/
│   │   │   ├── buyer-orders.js
│   │   │   ├── seller-orders.js
│   │   │   └── order-detail.js
│   │   ├── payments/
│   │   │   ├── payment.js
│   │   │   └── payment-status.js
│   │   ├── messages/
│   │   │   ├── messages.js
│   │   │   └── thread.js
│   │   ├── reviews/
│   │   │   └── reviews.js
│   │   ├── seller/
│   │   │   └── verification.js
│   │   └── admin/          # Admin dashboard pages
│   │       ├── dashboard.js
│   │       ├── users.js
│   │       ├── products.js
│   │       ├── logs.js
│   │       └── verifications.js
│   ├── services/           # API communication layer
│   │   ├── api.js          # HTTP client wrapper
│   │   ├── authService.js
│   │   ├── userService.js
│   │   ├── productService.js
│   │   ├── cartService.js
│   │   ├── orderService.js
│   │   ├── paymentService.js
│   │   ├── reviewService.js
│   │   ├── messageService.js
│   │   ├── notificationService.js
│   │   └── Services.js
│   ├── utils/              # Utility functions
│   │   ├── auth.js         # Auth state management
│   │   ├── storage.js      # localStorage wrapper
│   │   ├── validators.js   # Form validation
│   │   └── assets.js       # Asset URL builder
├── images/                 # Static images
└── Logo_bater/             # Brand assets
```

## Features by User Role

### Buyer
- Browse and search products
- View product details and reviews
- Add items to cart and checkout
- Pay via PayFast gateway
- View order history and status
- Leave product reviews
- Message sellers with questions
- Save wishlist
- Manage account profile

### Seller
- Create and manage product listings
- View product analytics
- Process buyer orders
- Track sales and inventory
- Respond to buyer messages
- Submit seller verification
- View payment settlements
- Manage seller profile

### Admin
- Dashboard with KPIs and analytics
- Manage users (suspend/activate)
- Moderate product listings
- Review seller verifications
- Monitor transactions and payments
- View admin action audit logs
- System-wide settings management

## Getting Started

### Prerequisites
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Internet connection
- Backend API running and accessible

### Installation

#### 1. Server Setup
```bash
cd htdocs
python -m http.server 8000
# or with PHP
php -S localhost:8000
```

#### 2. Configure API Base URL
Edit `js/config.js`:
```javascript
const API_BASE_URL = 'http://localhost/bater/public';
// For production
// const API_BASE_URL = 'https://api.bater.com';
```

#### 3. Open Application
```
http://localhost:8000
```

## Architecture Details

### SPA Routing

Hash-based routing in `js/app.js`:
```
#/                    → Home
#/login               → Login
#/register            → Register
#/products            → Product list
#/products/{id}       → Product detail
#/cart                → Shopping cart
#/orders              → Order history
#/admin/dashboard     → Admin panel
```

### Component Structure

Components are self-contained modules that export render functions:
```javascript
// js/components/navbar.js
export const navByRole = {
  buyer: () => { /* return HTML */ },
  seller: () => { /* return HTML */ },
  admin: () => { /* return HTML */ }
};
```

### Service Layer

Services handle all API communication:
```javascript
// js/services/productService.js
export const productService = {
  getAll: async () => await apiGet('/products'),
  getById: async (id) => await apiGet(`/products/${id}`),
  create: async (data) => await apiPost('/products', data),
  update: async (id, data) => await apiPut(`/products/${id}`, data)
};
```

### State Management

Application state is managed via:
1. **localStorage**: Persistent data (auth token, user preferences)
2. **Memory**: Session data (cart items, UI state)
3. **Backend**: Single source of truth for user/business data

### Authentication Flow

```
1. User enters credentials on login page
2. LoginPage → authService.login(email, password)
3. API returns user data and session established
4. Auth state stored in localStorage
5. App redirects to appropriate dashboard based on role
6. Auth middleware validates session on each API call
7. On logout: session cleared, localStorage cleaned
```

## Styling System

### CSS Variables
Centralized design tokens in `css/variables.css`:
```css
:root {
  --primary-color: #3498db;
  --secondary-color: #2ecc71;
  --danger-color: #e74c3c;
  --spacing-unit: 8px;
  --border-radius: 4px;
}
```

### Responsive Design
Mobile-first approach with breakpoints:
```css
/* Mobile: < 640px */
/* Tablet: 640px - 1024px */
/* Desktop: > 1024px */
```

### Component CSS Classes
```css
.btn                   /* Base button */
.btn.btn-primary       /* Primary button */
.btn.btn-secondary     /* Secondary button */
.card                  /* Card container */
.form-group            /* Form input group */
.alert                 /* Alert messages */
.loading               /* Loading state */
```

## API Communication

### apiCall Function
```javascript
import { apiCall } from './services/api.js';

// GET request
const products = await apiCall('/products', { method: 'GET' });

// POST request with data
const result = await apiCall('/orders', {
  method: 'POST',
  body: { items: [...] }
});

// Automatic error handling and notifications
```

### Request/Response Format

Request:
```json
{
  "method": "POST",
  "endpoint": "/products",
  "body": {
    "name": "Product Name",
    "price": 299.99
  }
}
```

Response:
```json
{
  "success": true,
  "data": { "id": 123, "name": "Product Name" },
  "message": "Product created successfully"
}
```

## Forms & Validation

### Form Validation
```javascript
import { validateEmail, validatePassword } from './utils/validators.js';

const isValidEmail = validateEmail(email);
const isValidPassword = validatePassword(password);
```

### Form Submission Pattern
```javascript
// 1. Get form values
const formData = new FormData(form);

// 2. Validate
if (!validateForm(formData)) return;

// 3. Submit
const response = await authService.login(data);

// 4. Handle response
if (response.success) {
  showNotification('Login successful', 'success');
  window.location.hash = '#/dashboard';
}
```

## Notifications

Toast notifications for user feedback:
```javascript
import { showNotification } from './components/notifications.js';

showNotification('Operation successful!', 'success', 3000);
showNotification('An error occurred', 'error', 0); // Persistent
```

Notification types: `success`, `error`, `warning`, `info`

## Performance Optimization

✅ **Implemented**
- No external dependencies (vanilla JavaScript)
- Minimal HTTP requests via service batching
- CSS minification
- Image optimization
- Lazy loading of components
- LocalStorage caching

✅ **Best Practices**
- Bundle splitting by page
- Defer non-critical assets
- Use WebP for images with fallbacks
- Compress static assets
- Enable browser caching

## Browser Support

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## Development Workflow

### Adding a New Page

1. Create page module in `js/pages/`:
```javascript
// js/pages/newpage.js
export async function newPage() {
  const data = await service.getData();
  return `<div>${data}</div>`;
}

export function initNewPage() {
  // Attach event listeners
}
```

2. Import in `js/app.js`:
```javascript
import { newPage, initNewPage } from './pages/newpage.js';
```

3. Add route:
```javascript
case '#/newpage':
  content = await newPage();
  init = initNewPage;
  break;
```

### Adding a New Component

1. Create component in `js/components/`:
```javascript
// js/components/mycomponent.js
export function myComponent(data) {
  return `<div class="component">${data}</div>`;
}
```

2. Import and use in pages:
```javascript
import { myComponent } from '../../components/mycomponent.js';

const html = myComponent(data);
```

## Debugging

### Enable Debug Logging
Edit `js/config.js`:
```javascript
export const DEBUG = true;
```

### Common Issues

**Blank Page**
- Check browser console for errors
- Verify API_BASE_URL is correct
- Ensure backend is running

**API Errors**
- Check network tab in DevTools
- Verify session is active
- Check for CORS issues

**Styling Issues**
- Clear browser cache
- Check CSS file is loaded
- Verify CSS variables are defined

## Deployment

### Build for Production

1. Minify CSS and JavaScript
```bash
# Using UglifyJS and clean-css
uglifyjs js/*.js -o js/app.min.js
cleancss css/styles.css -o css/styles.min.css
```

2. Update `index.html` to reference minified files:
```html
<link rel="stylesheet" href="css/styles.min.css">
<script src="js/app.min.js"></script>
```

3. Update API URL in `js/config.js`:
```javascript
const API_BASE_URL = 'https://api.production.com';
```

### Hosting Options
- Static hosting (Netlify, Vercel, GitHub Pages)
- Web server (Apache, Nginx) with PHP backend
- Container deployment (Docker)

### Production Checklist
- [ ] Minify all assets
- [ ] Enable gzip compression
- [ ] Set correct API endpoints
- [ ] Test all user flows
- [ ] Verify mobile responsiveness
- [ ] Enable HTTPS
- [ ] Set up error logging
- [ ] Configure CDN for images
- [ ] Test with various browsers
- [ ] Load testing and optimization

## Security Considerations

✅ **Implemented**
- Backend session validation
- CSRF protection via sessions
- Input sanitization via backend
- XSS prevention via DOM APIs

✅ **Recommendations**
- Use HTTPS in production
- Implement Content Security Policy (CSP)
- Regular security audits
- Keep dependencies updated
- Monitor for unauthorized access
- Implement rate limiting on API

## Performance Metrics

Target metrics:
- First Contentful Paint (FCP): < 1.5s
- Largest Contentful Paint (LCP): < 2.5s
- Cumulative Layout Shift (CLS): < 0.1
- Time to Interactive (TTI): < 3.5s

## Contributing

1. Create feature branch
2. Make changes to pages/components
3. Test thoroughly
4. Submit pull request

## License

Proprietary - Bater Development Team

## Support

For issues and feature requests, contact the development team.

---

**Last Updated**: June 2024  
**Version**: 1.0.0  
**Frontend Stack**: Vanilla JS (ES6+), HTML5, CSS3  
**Maintainer**: Bater Development Team
