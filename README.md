# Metrics Service - Technical Documentation

## Table of Contents
1. [Service Overview](#service-overview)
2. [Technology Stack](#technology-stack)
3. [Architecture](#architecture)
4. [Installation & Setup](#installation--setup)
5. [Configuration](#configuration)
6. [API Reference](#api-reference)
7. [Deployment Guide](#deployment-guide)
8. [User Manual](#user-manual)
9. [Update Manual](#update-manual)
10. [Monitoring & Troubleshooting](#monitoring--troubleshooting)
11. [Security Considerations](#security-considerations)
12. [Testing](#testing)

## Service Overview

The Metrics Service is a comprehensive monitoring and analytics microservice responsible for tracking, collecting, and analyzing various metrics across the NydArt Advisor application. It provides real-time insights into AI request performance, user engagement, sales analytics, and system performance.

### Key Features
- **AI Request Tracking**: Monitor AI analysis requests, response times, and usage patterns
- **User Engagement Analytics**: Track user behavior, session data, and feature usage
- **Sales Analytics**: Monitor payment processing, revenue metrics, and subscription data
- **Performance Monitoring**: System performance metrics, response times, and resource utilization
- **Real-time Dashboards**: Prometheus metrics for monitoring and alerting
- **Data Visualization**: Analytics endpoints for chart and report generation
- **Caching**: Redis integration for high-performance data access

### Service Responsibilities
- Collect and store metrics from all application services
- Provide analytics and reporting capabilities
- Monitor system performance and health
- Generate insights for business intelligence
- Support monitoring and alerting systems
- Maintain historical data for trend analysis

## Technology Stack

### Core Technologies
- **Runtime**: Node.js 18+
- **Framework**: Express.js 4.18.2
- **Language**: JavaScript (ES6+)

### Database & Storage
- **MongoDB**: Primary data storage with Mongoose ODM
- **Redis**: Caching and session storage
- **Prometheus**: Metrics collection and monitoring

### Monitoring & Analytics
- **Prometheus Client**: Metrics collection
- **Winston**: Logging framework
- **Morgan**: HTTP request logging
- **Moment.js**: Date/time manipulation

### Security & Performance
- **Helmet**: Security headers
- **CORS**: Cross-origin resource sharing
- **Rate Limiting**: Request throttling
- **Compression**: Response compression
- **JWT**: Authentication middleware

### Development & Testing
- **Mocha**: Test framework
- **Chai**: Assertion library
- **Sinon**: Mocking and stubbing
- **Supertest**: HTTP testing
- **NYC**: Code coverage
- **Jest**: Alternative test framework

### Utilities
- **Axios**: HTTP client for service communication
- **Lodash**: Utility functions
- **Express Validator**: Input validation
- **dotenv**: Environment variable management

## Architecture

### Service Structure
```
metrics_service/
├── src/
│   ├── server.js              # Main application entry point
│   ├── routes/
│   │   ├── metrics.js         # Core metrics endpoints
│   │   ├── aiTracking.js      # AI request tracking
│   │   ├── analytics.js       # User engagement analytics
│   │   └── performance.js     # Performance monitoring
│   ├── models/
│   │   ├── AIRequest.js       # AI request data model
│   │   ├── UserEngagement.js  # User engagement model
│   │   ├── SalesAnalytics.js  # Sales data model
│   │   └── PerformanceMetrics.js # Performance data model
│   ├── middleware/
│   │   ├── auth.js           # Authentication middleware
│   │   └── errorHandler.js   # Error handling middleware
│   ├── config/
│   │   ├── database.js       # MongoDB configuration
│   │   └── redis.js          # Redis configuration
│   ├── utils/
│   │   └── logger.js         # Logging utility
│   └── test/                 # Test suite
├── package.json
└── .env                      # Environment variables
```

### Data Flow
1. **Data Collection**: Services send metrics data via HTTP requests
2. **Validation**: Input validation and sanitization
3. **Storage**: Data stored in MongoDB with Redis caching
4. **Processing**: Real-time aggregation and analysis
5. **Exposure**: Metrics exposed via Prometheus and REST APIs
6. **Visualization**: Data available for dashboards and reports

### Service Dependencies
- **Frontend Service**: Sends user engagement metrics
- **AI Service**: Sends AI request metrics
- **Payment Service**: Sends sales and payment metrics
- **Auth Service**: Provides authentication for protected endpoints
- **Database Service**: May query user data for analytics

## Installation & Setup

### Prerequisites
- Node.js 18+ installed
- npm or yarn package manager
- MongoDB instance (local or cloud)
- Redis instance (local or cloud)

### Installation Steps

1. **Clone and Navigate**
   ```bash
   cd metrics_service
   ```

2. **Install Dependencies**
   ```bash
   npm install
   ```

3. **Environment Configuration**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

4. **Database Setup**
   ```bash
   # Ensure MongoDB is running
   # Ensure Redis is running
   ```

5. **Verify Installation**
   ```bash
   npm test
   ```

### Database Setup

#### MongoDB Setup
1. **Local MongoDB**
   ```bash
   # Install MongoDB locally
   # Start MongoDB service
   mongod --dbpath /path/to/data
   ```

2. **MongoDB Atlas (Cloud)**
   - Create account at https://cloud.mongodb.com
   - Create cluster
   - Get connection string
   - Add to `.env` file

#### Redis Setup
1. **Local Redis**
   ```bash
   # Install Redis locally
   # Start Redis service
   redis-server
   ```

2. **Redis Cloud**
   - Create account at https://redis.com
   - Create database
   - Get connection details
   - Add to `.env` file

## Configuration

### Environment Variables

#### Server Configuration
```env
PORT=5005
NODE_ENV=production
```

#### Database Configuration
```env
# MongoDB Configuration
MONGODB_URI=mongodb://localhost:27017/metrics_service
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/metrics_service

# Redis Configuration
REDIS_URL=redis://localhost:6379
REDIS_URL=redis://username:password@host:port
```

#### Security Configuration
```env
# JWT Configuration
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRES_IN=24h

# Rate Limiting
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100
```

#### Service URLs
```env
FRONTEND_URL=https://your-frontend-domain.com
CLIENT_URL=https://your-client-domain.com
```

#### Monitoring Configuration
```env
# Prometheus Configuration
ENABLE_METRICS=true
METRICS_PORT=9090

# Logging Configuration
LOG_LEVEL=info
LOG_FILE=logs/metrics-service.log
```

### Configuration Files

#### Database Configuration (`src/config/database.js`)
- MongoDB connection setup
- Connection pooling
- Error handling

#### Redis Configuration (`src/config/redis.js`)
- Redis connection setup
- Cache configuration
- Session storage setup

## API Reference

### Base URL
```
http://localhost:5005 (development)
https://your-metrics-service.com (production)
```

### Authentication
All API endpoints require JWT authentication via Authorization header:
```http
Authorization: Bearer <jwt_token>
```

### Endpoints

#### Health Check
```http
GET /health
```
**Response:**
```json
{
  "status": "OK",
  "service": "Metrics Service",
  "timestamp": "2024-01-15T10:30:00Z",
  "environment": "production",
  "version": "1.0.0"
}
```

#### Prometheus Metrics
```http
GET /metrics
```
**Response:** Prometheus-formatted metrics

#### AI Request Tracking

##### Track AI Request
```http
POST /api/ai-tracking/request
Content-Type: application/json

{
  "userId": "user123",
  "imageUrl": "https://example.com/image.jpg",
  "requestType": "art_analysis",
  "model": "gpt-4-vision",
  "timestamp": "2024-01-15T10:30:00Z"
}
```

##### Get AI Request Stats
```http
GET /api/ai-tracking/stats?period=daily&startDate=2024-01-01&endDate=2024-01-15
```

#### User Engagement Analytics

##### Track User Activity
```http
POST /api/analytics/activity
Content-Type: application/json

{
  "userId": "user123",
  "action": "login",
  "page": "/dashboard",
  "sessionId": "session123",
  "timestamp": "2024-01-15T10:30:00Z"
}
```

##### Get User Engagement Stats
```http
GET /api/analytics/engagement?period=weekly&startDate=2024-01-01&endDate=2024-01-15
```

#### Sales Analytics

##### Track Payment
```http
POST /api/analytics/payment
Content-Type: application/json

{
  "userId": "user123",
  "amount": 29.99,
  "currency": "USD",
  "paymentMethod": "stripe",
  "subscriptionType": "premium",
  "timestamp": "2024-01-15T10:30:00Z"
}
```

##### Get Sales Stats
```http
GET /api/analytics/sales?period=monthly&startDate=2024-01-01&endDate=2024-01-31
```

#### Performance Monitoring

##### Track Performance Metric
```http
POST /api/performance/metric
Content-Type: application/json

{
  "service": "ai_service",
  "endpoint": "/analyze",
  "responseTime": 1500,
  "statusCode": 200,
  "timestamp": "2024-01-15T10:30:00Z"
}
```

##### Get Performance Stats
```http
GET /api/performance/stats?service=ai_service&period=hourly&startDate=2024-01-15T00:00:00Z&endDate=2024-01-15T23:59:59Z
```

### Error Responses

#### Authentication Error (401)
```json
{
  "error": "Unauthorized",
  "message": "Invalid or missing token"
}
```

#### Validation Error (400)
```json
{
  "error": "Validation Error",
  "message": "Invalid input data",
  "details": ["userId is required"]
}
```

#### Server Error (500)
```json
{
  "error": "Internal Server Error",
  "message": "An unexpected error occurred"
}
```

## Deployment Guide

### Production Deployment

#### Environment Setup
1. **Set Production Environment**
   ```bash
   NODE_ENV=production
   ```

2. **Configure Production Variables**
   - Use production database URLs
   - Set production service URLs
   - Configure CORS for production domains

3. **SSL/TLS Configuration**
   - Ensure HTTPS endpoints
   - Configure SSL certificates
   - Set secure headers

#### Deployment Options

##### Docker Deployment
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 5005
CMD ["npm", "start"]
```

##### PM2 Deployment
```bash
npm install -g pm2
pm2 start src/server.js --name "metrics-service"
pm2 save
pm2 startup
```

##### Cloud Platform Deployment
- **Render.com**: Connect GitHub repository
- **Heroku**: Use Heroku CLI or GitHub integration
- **AWS**: Use Elastic Beanstalk or ECS
- **Google Cloud**: Use App Engine or Cloud Run

#### Health Checks
```bash
# Check service health
curl https://your-service.com/health

# Check metrics
curl https://your-service.com/metrics
```

### Monitoring Setup

#### Prometheus Configuration
```yaml
scrape_configs:
  - job_name: 'metrics-service'
    static_configs:
      - targets: ['localhost:5005']
    metrics_path: '/metrics'
    scrape_interval: 15s
```

#### Grafana Dashboard
```json
{
  "dashboard": {
    "title": "NydArt Advisor Metrics",
    "panels": [
      {
        "title": "AI Requests per Hour",
        "type": "graph",
        "targets": [
          {
            "expr": "rate(ai_requests_total[1h])",
            "legendFormat": "AI Requests/sec"
          }
        ]
      },
      {
        "title": "User Engagement",
        "type": "graph",
        "targets": [
          {
            "expr": "rate(user_activities_total[1h])",
            "legendFormat": "User Activities/sec"
          }
        ]
      }
    ]
  }
}
```

#### Alert Rules
```yaml
groups:
  - name: metrics-service
    rules:
      - alert: MetricsServiceDown
        expr: up{job="metrics-service"} == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "Metrics service is down"
      
      - alert: HighErrorRate
        expr: rate(http_requests_total{status=~"5.."}[5m]) > 0.1
        for: 2m
        labels:
          severity: warning
        annotations:
          summary: "High error rate detected"
```

## User Manual

### For Developers

#### Sending Metrics Data
```javascript
// Track AI request
const response = await fetch('/api/ai-tracking/request', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${token}`
  },
  body: JSON.stringify({
    userId: 'user123',
    imageUrl: 'https://example.com/image.jpg',
    requestType: 'art_analysis',
    model: 'gpt-4-vision',
    timestamp: new Date().toISOString()
  })
});

// Track user activity
const response = await fetch('/api/analytics/activity', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${token}`
  },
  body: JSON.stringify({
    userId: 'user123',
    action: 'login',
    page: '/dashboard',
    sessionId: 'session123',
    timestamp: new Date().toISOString()
  })
});
```

#### Retrieving Analytics
```javascript
// Get AI request stats
const response = await fetch('/api/ai-tracking/stats?period=daily&startDate=2024-01-01&endDate=2024-01-15', {
  headers: {
    'Authorization': `Bearer ${token}`
  }
});

const stats = await response.json();
console.log('AI Request Stats:', stats);
```

#### Error Handling
```javascript
try {
  const response = await fetch('/api/analytics/activity', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${token}`
    },
    body: JSON.stringify(data)
  });
  
  if (!response.ok) {
    const error = await response.json();
    console.error('Metrics tracking failed:', error.message);
  }
} catch (error) {
  console.error('Request failed:', error);
}
```

### For System Administrators

#### Service Management
```bash
# Start service
npm start

# Start in development mode
npm run dev

# Run tests
npm test

# Check service health
curl http://localhost:5005/health
```

#### Database Management
```bash
# Check MongoDB connection
mongo metrics_service --eval "db.stats()"

# Check Redis connection
redis-cli ping

# Monitor database performance
mongo metrics_service --eval "db.currentOp()"
```

#### Log Monitoring
```bash
# View logs
tail -f logs/metrics-service.log

# Monitor errors
grep "ERROR" logs/metrics-service.log

# Monitor performance
grep "responseTime" logs/metrics-service.log
```

## Update Manual

### Version Updates

#### Minor Updates
1. **Backup Data**
   ```bash
   # Backup MongoDB
   mongodump --db metrics_service --out backup/
   
   # Backup Redis
   redis-cli BGSAVE
   ```

2. **Update Dependencies**
   ```bash
   npm update
   ```

3. **Test Changes**
   ```bash
   npm test
   ```

4. **Restart Service**
   ```bash
   pm2 restart metrics-service
   ```

#### Major Updates
1. **Review Changelog**
   - Check breaking changes
   - Review new features
   - Verify compatibility

2. **Staging Deployment**
   - Deploy to staging environment
   - Run full test suite
   - Verify all metrics collection works

3. **Production Deployment**
   - Schedule maintenance window
   - Deploy with rollback plan
   - Monitor closely after deployment

### Configuration Updates

#### Database Changes
1. **Update Connection Strings**
   ```bash
   # Update MongoDB URI
   MONGODB_URI=new_mongodb_connection_string
   
   # Update Redis URL
   REDIS_URL=new_redis_connection_string
   ```

2. **Test Connections**
   ```bash
   curl http://localhost:5005/health
   ```

3. **Restart Service**
   ```bash
   pm2 restart metrics-service
   ```

#### Monitoring Changes
1. **Update Prometheus Configuration**
   ```yaml
   scrape_configs:
     - job_name: 'metrics-service'
       static_configs:
         - targets: ['new-service-url:5005']
   ```

2. **Update Alert Rules**
   ```yaml
   groups:
     - name: metrics-service
       rules:
         - alert: NewAlert
           expr: new_metric > threshold
   ```

## Monitoring & Troubleshooting

### Key Metrics

#### Performance Metrics
- **Response Time**: Average API response time
- **Throughput**: Requests per second
- **Error Rate**: Percentage of failed requests
- **Database Performance**: Query response times

#### Business Metrics
- **AI Request Volume**: Number of AI analyses per time period
- **User Engagement**: Active users, session duration
- **Sales Performance**: Revenue, conversion rates
- **Feature Usage**: Most used features and pages

### Common Issues

#### Database Connection Issues
**Symptoms:**
- 500 errors on API endpoints
- Database connection timeouts
- High response times

**Solutions:**
1. Check MongoDB connection string
2. Verify network connectivity
3. Check database server status
4. Review connection pool settings

#### Redis Connection Issues
**Symptoms:**
- Cache misses
- Session storage failures
- Performance degradation

**Solutions:**
1. Check Redis connection string
2. Verify Redis server status
3. Check memory usage
4. Review cache configuration

#### High Memory Usage
**Symptoms:**
- Service crashes
- Slow response times
- Out of memory errors

**Solutions:**
1. Monitor memory usage
2. Optimize database queries
3. Implement data archiving
4. Scale horizontally

### Debugging Tools

#### Log Analysis
```bash
# View real-time logs
tail -f logs/metrics-service.log

# Search for specific errors
grep "ERROR" logs/metrics-service.log | tail -20

# Monitor performance
grep "responseTime" logs/metrics-service.log | tail -10
```

#### Health Checks
```bash
# Basic health check
curl http://localhost:5005/health

# Detailed metrics
curl http://localhost:5005/metrics

# Database health
curl http://localhost:5005/health/database
```

#### Performance Testing
```bash
# Load test
ab -n 1000 -c 10 http://localhost:5005/health

# Stress test
ab -n 10000 -c 100 http://localhost:5005/health
```

## Security Considerations

### Authentication & Authorization
- JWT-based authentication for all API endpoints
- Token validation and expiration
- Role-based access control (if implemented)

### Data Protection
- Input validation and sanitization
- SQL injection prevention
- XSS protection
- Data encryption in transit and at rest

### API Security
- Rate limiting to prevent abuse
- CORS configuration for allowed origins
- Input validation and sanitization
- Secure headers configuration

### Monitoring Security
- Audit logging for all operations
- Access control for metrics endpoints
- Secure storage of sensitive data
- Regular security updates

### Compliance
- GDPR compliance for user data
- Data retention policies
- Privacy protection measures
- Regular security audits

## Testing

### Test Categories

#### Unit Tests
- Service function testing
- Data model validation
- Utility function testing
- Error handling tests

#### Integration Tests
- API endpoint testing
- Database integration
- Redis integration
- External service integration

#### Performance Tests
- Load testing
- Stress testing
- Database performance
- Response time testing

### Running Tests

#### Full Test Suite
```bash
npm test
```

#### Specific Test Categories
```bash
# Unit tests only
npm run test:unit

# Integration tests only
npm run test:integration

# Performance tests only
npm run test:performance
```

#### Test Coverage
```bash
npm run test:coverage
```

### Test Configuration

#### Test Environment
```env
NODE_ENV=test
MONGODB_URI=mongodb://localhost:27017/metrics_service_test
REDIS_URL=redis://localhost:6379/1
```

#### Mock Services
- External APIs mocked for testing
- Database operations mocked
- Redis operations mocked

### Continuous Integration

#### GitHub Actions
```yaml
name: Metrics Service Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    services:
      mongodb:
        image: mongo:5.0
        ports:
          - 27017:27017
      redis:
        image: redis:6.2
        ports:
          - 6379:6379
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '18'
      - run: npm ci
      - run: npm test
      - run: npm run test:coverage
```

#### Test Reports
- Coverage reports generated
- Test results published
- Performance metrics tracked

---

## Support & Maintenance

### Documentation Updates
- Keep this documentation current
- Update API examples
- Maintain troubleshooting guides

### Regular Maintenance
- Monitor service performance
- Update dependencies regularly
- Review security configurations
- Backup data regularly

### Contact Information
- Technical issues: Create GitHub issue
- Security concerns: Contact security team
- General questions: Check documentation first

---

*Last Updated: January 2024*
*Version: 1.0.0*
