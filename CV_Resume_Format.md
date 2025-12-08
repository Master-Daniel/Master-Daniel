# Software Engineer - FreePass CDN Platform

## Project Summary
Developed and maintained a comprehensive CDN and content management platform with 1,415+ commits across 9 microservices, focusing on M2M edge connectivity, content management, authentication, and merchant portal development.

---

## Key Features Developed

### M2M Edge Connectivity Platform
- Built complete IoT connectivity platform supporting MQTT and HTTP protocols
- Implemented asset management system (assets, asset groups, asset types) with CRUD operations
- Developed flow integration system for device-to-cloud communication
- Created topic schema management with validation and compatibility checking
- Built real-time monitoring dashboard for MQTT connections and data flow
- **Technologies**: NestJS, MongoDB, JWT, Swagger, MQTT Protocol

### Content Catalog & Media Management System
- Developed comprehensive playlist management system with multi-part support and monetization
- Implemented archive catalog with encryption/compression capabilities
- Built restoration system with scheduled tasks and history tracking
- Created media library with resumable uploads (TUS protocol) and progress tracking
- Migrated file storage from AWS S3 to Cloudinary for improved performance
- Implemented subtitle management (SRT files) and video player integration
- **Technologies**: React, Next.js, MongoDB, Cloudinary, TUS Protocol, EventSource

### Authentication & Authorization System
- Built complete authentication microservice with multi-step registration flow
- Implemented OAuth 2.0 SSO integration (Google, Microsoft)
- Developed Role-Based Access Control (RBAC) system with permission management
- Created team management system with email-based invitations
- Implemented activity logging and audit trail system
- Built account lockout mechanism with failed login attempt tracking
- **Technologies**: NestJS, Passport.js, JWT, OAuth 2.0, AWS SES, MongoDB

### Merchant Portal Frontend (1,009 commits)
- Developed comprehensive merchant dashboard with analytics and monitoring
- Built M2M edge connectivity UI with asset management and flow integration
- Created content management interface with drag-and-drop playlist builder
- Implemented dark mode support across all application pages
- Developed responsive, accessible UI with ARIA labels and keyboard navigation
- Built advanced data tables with sorting, filtering, pagination, and row selection
- Implemented real-time file upload with progress tracking using EventSource
- **Technologies**: React 19, Next.js 15, TypeScript, Tailwind CSS, React Query, Zustand, Chart.js

### Multimedia Streaming Service
- Built scalable streaming service with media upload and retrieval
- Implemented playlist service with content review workflow
- Created multi-connection database architecture for service isolation
- Developed repository pattern for data access layer abstraction
- **Technologies**: NestJS, MongoDB, Mongoose, JWT

### Device Onboarding & Settings
- Developed device authentication and onboarding API
- Implemented real-time device status tracking
- Built secure passkey generation and management system
- **Technologies**: Node.js, Express, MongoDB, Docker

---

## Technical Stack

### Frontend
- **Frameworks**: React 19, Next.js 15, TypeScript 5
- **Styling**: Tailwind CSS, DaisyUI, CSS Modules
- **State Management**: Zustand, React Query (TanStack Query), Context API
- **Forms**: React Hook Form, Zod validation
- **UI Components**: NextUI, Custom component library
- **Data Visualization**: Chart.js, React Charts
- **File Handling**: React Dropzone, TUS JS Client

### Backend
- **Framework**: NestJS 10/11, Node.js 18/22
- **Database**: MongoDB, MongoDB Atlas, Mongoose, TypeORM
- **Authentication**: JWT, Passport.js, OAuth 2.0, bcrypt
- **API Documentation**: Swagger/OpenAPI
- **Validation**: Class Validator, Class Transformer, Zod

### Cloud Services & Infrastructure
- **AWS**: SES (Email), S3 (migrated to Cloudinary)
- **Cloudinary**: Image/video management and transformations
- **Cloudflare**: CDN and edge services
- **Docker**: Multi-stage builds, containerization
- **Kubernetes**: Container orchestration
- **NGINX**: Ingress controller

### Development Tools
- **Testing**: Jest, Supertest, E2E testing
- **Code Quality**: ESLint, Prettier, TypeScript strict mode
- **Version Control**: Git, feature branch workflow
- **Documentation**: Swagger, Markdown

---

## Key Achievements

- **1,415+ Commits** across 9 microservices in production
- **1,009 Commits** in merchant portal (largest individual contribution)
- Built **complete M2M IoT platform** from scratch
- Successfully **migrated file storage** from AWS S3 to Cloudinary
- Implemented **comprehensive authentication system** with SSO
- Developed **scalable microservices architecture**
- Created **responsive, accessible UI** with dark mode support
- Implemented **real-time features** (EventSource, progress tracking)

---

## Technical Skills

### Languages & Frameworks
TypeScript, JavaScript (ES6+), Node.js, React, Next.js, NestJS, Express

### Databases & ORMs
MongoDB, MongoDB Atlas, Mongoose, TypeORM, Database Design, Multi-connection Architecture

### Authentication & Security
JWT, OAuth 2.0, Passport.js, RBAC, bcrypt, Security Best Practices, Audit Logging

### Cloud & DevOps
AWS (SES, S3), Cloudinary, Docker, Kubernetes, NGINX, Cloudflare, CI/CD

### Frontend Technologies
React Hooks, Server-Side Rendering, State Management, Form Validation, Data Visualization, Responsive Design, Accessibility (WCAG)

### Testing & Quality
Unit Testing, Integration Testing, E2E Testing, Code Review, ESLint, Prettier

---

## Architecture & Design Patterns

- **Microservices Architecture**: Service isolation and inter-service communication
- **Repository Pattern**: Data access layer abstraction
- **Module Pattern**: NestJS modular architecture
- **DTO Pattern**: Data transfer objects for validation
- **Service Layer Pattern**: Business logic separation
- **RESTful API Design**: REST principles and best practices

---

*This summary highlights key contributions and technical expertise in building a production-grade CDN and content management platform.*

