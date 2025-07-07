# Deployment Guide - Voice Task Manager

This guide provides detailed instructions for deploying the Voice Task Manager application to various platforms.

## 🏗️ Deployment Architecture

The application consists of two main components:
- **Backend API** (Flask): Handles data management and API endpoints
- **Frontend App** (React): Provides the user interface and voice interaction

## 🚀 Quick Deployment Options

### Option 1: Heroku (Recommended for beginners)

#### Backend Deployment (Heroku)

1. **Prepare the backend**:
   ```bash
   cd voice_task_manager_api
   ```

2. **Create Procfile**:
   ```bash
   echo "web: python src/main.py" > Procfile
   ```

3. **Update main.py for Heroku**:
   ```python
   import os
   # Add this to main.py
   port = int(os.environ.get('PORT', 5000))
   app.run(host='0.0.0.0', port=port)
   ```

4. **Deploy to Heroku**:
   ```bash
   heroku create your-app-name-api
   git push heroku main
   heroku config:set FLASK_ENV=production
   ```

#### Frontend Deployment (Vercel)

1. **Prepare the frontend**:
   ```bash
   cd voice-task-manager-frontend
   ```

2. **Update API URL**:
   Create `.env.production`:
   ```
   VITE_API_URL=https://your-app-name-api.herokuapp.com
   ```

3. **Deploy to Vercel**:
   ```bash
   npm install -g vercel
   vercel --prod
   ```

### Option 2: DigitalOcean App Platform

#### Backend Deployment

1. **Create app.yaml**:
   ```yaml
   name: voice-task-manager-api
   services:
   - name: api
     source_dir: /
     github:
       repo: your-username/voice-task-manager
       branch: main
     run_command: python src/main.py
     environment_slug: python
     instance_count: 1
     instance_size_slug: basic-xxs
     envs:
     - key: FLASK_ENV
       value: production
   ```

2. **Deploy via DigitalOcean dashboard** or CLI

#### Frontend Deployment

1. **Create app.yaml for frontend**:
   ```yaml
   name: voice-task-manager-frontend
   static_sites:
   - name: frontend
     source_dir: /
     github:
       repo: your-username/voice-task-manager-frontend
       branch: main
     build_command: npm run build
     output_dir: dist
     envs:
     - key: VITE_API_URL
       value: https://your-api-url.ondigitalocean.app
   ```

### Option 3: AWS (Advanced)

#### Backend (AWS Elastic Beanstalk)

1. **Install EB CLI**:
   ```bash
   pip install awsebcli
   ```

2. **Initialize and deploy**:
   ```bash
   cd voice_task_manager_api
   eb init
   eb create production
   eb deploy
   ```

#### Frontend (AWS S3 + CloudFront)

1. **Build the application**:
   ```bash
   cd voice-task-manager-frontend
   npm run build
   ```

2. **Deploy to S3**:
   ```bash
   aws s3 sync dist/ s3://your-bucket-name --delete
   ```

3. **Configure CloudFront** for global CDN distribution

## 🐳 Docker Deployment

### Backend Dockerfile

Create `voice_task_manager_api/Dockerfile`:
```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY src/ ./src/

EXPOSE 5000

CMD ["python", "src/main.py"]
```

### Frontend Dockerfile

Create `voice-task-manager-frontend/Dockerfile`:
```dockerfile
FROM node:20-alpine as build

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80
```

### Docker Compose

Create `docker-compose.yml`:
```yaml
version: '3.8'

services:
  backend:
    build: ./voice_task_manager_api
    ports:
      - "5000:5000"
    environment:
      - FLASK_ENV=production
    volumes:
      - ./data:/app/data

  frontend:
    build: ./voice-task-manager-frontend
    ports:
      - "80:80"
    depends_on:
      - backend
    environment:
      - VITE_API_URL=http://localhost:5000
```

Run with:
```bash
docker-compose up -d
```

## 🔧 Environment Configuration

### Backend Environment Variables

Create `.env` file in backend directory:
```env
FLASK_ENV=production
DATABASE_URL=sqlite:///tasks.db
SECRET_KEY=your-secret-key-here
CORS_ORIGINS=https://your-frontend-domain.com
```

### Frontend Environment Variables

Create `.env.production` in frontend directory:
```env
VITE_API_URL=https://your-backend-domain.com
VITE_APP_TITLE=Voice Task Manager
```

## 🗄️ Database Setup

### SQLite (Development)
- Automatically created on first run
- File stored in backend directory

### PostgreSQL (Production)

1. **Install PostgreSQL**:
   ```bash
   pip install psycopg2-binary
   ```

2. **Update database URL**:
   ```env
   DATABASE_URL=postgresql://username:password@host:port/database
   ```

3. **Run migrations**:
   ```bash
   python -c "from src.main import db; db.create_all()"
   ```

## 🔒 Security Configuration

### Production Security Checklist

- [ ] Set strong SECRET_KEY
- [ ] Configure CORS for specific domains
- [ ] Use HTTPS for all communications
- [ ] Set up database backups
- [ ] Configure rate limiting
- [ ] Enable logging and monitoring

### HTTPS Setup

#### Using Let's Encrypt (Certbot)
```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d your-domain.com
```

#### Using Cloudflare
1. Point domain to Cloudflare
2. Enable SSL/TLS encryption
3. Set SSL mode to "Full (strict)"

## 📊 Monitoring and Logging

### Application Monitoring

Add to backend `main.py`:
```python
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

@app.before_request
def log_request_info():
    logger.info('Request: %s %s', request.method, request.url)
```

### Health Check Endpoints

Add to backend:
```python
@app.route('/health')
def health_check():
    return {'status': 'healthy', 'timestamp': datetime.utcnow().isoformat()}
```

## 🚨 Troubleshooting

### Common Issues

#### CORS Errors
- Ensure backend CORS is configured for frontend domain
- Check that API URL in frontend matches backend URL

#### Voice Recognition Not Working
- Ensure HTTPS is enabled (required for Web Speech API)
- Check browser compatibility
- Verify microphone permissions

#### Database Connection Issues
- Check DATABASE_URL format
- Ensure database server is running
- Verify network connectivity

#### Build Failures
- Check Node.js and Python versions
- Clear npm/pip cache
- Verify all dependencies are installed

### Debug Mode

Enable debug mode for troubleshooting:

Backend:
```python
app.run(debug=True, host='0.0.0.0', port=5000)
```

Frontend:
```bash
npm run dev
```

## 📈 Performance Optimization

### Backend Optimization
- Use production WSGI server (Gunicorn)
- Enable database connection pooling
- Implement caching for frequent queries
- Use CDN for static assets

### Frontend Optimization
- Enable gzip compression
- Implement code splitting
- Optimize images and assets
- Use service workers for caching

### Example Gunicorn Configuration

Create `gunicorn.conf.py`:
```python
bind = "0.0.0.0:5000"
workers = 4
worker_class = "sync"
timeout = 30
keepalive = 2
max_requests = 1000
max_requests_jitter = 100
```

Run with:
```bash
gunicorn -c gunicorn.conf.py src.main:app
```

## 🔄 CI/CD Pipeline

### GitHub Actions Example

Create `.github/workflows/deploy.yml`:
```yaml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy-backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Deploy to Heroku
        uses: akhileshns/heroku-deploy@v3.12.12
        with:
          heroku_api_key: ${{secrets.HEROKU_API_KEY}}
          heroku_app_name: "your-app-name-api"
          heroku_email: "your-email@example.com"
          appdir: "voice_task_manager_api"

  deploy-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{secrets.VERCEL_TOKEN}}
          vercel-org-id: ${{secrets.ORG_ID}}
          vercel-project-id: ${{secrets.PROJECT_ID}}
          working-directory: ./voice-task-manager-frontend
```

## 📋 Deployment Checklist

### Pre-Deployment
- [ ] All tests passing
- [ ] Environment variables configured
- [ ] Database migrations ready
- [ ] HTTPS certificates obtained
- [ ] Domain DNS configured

### Post-Deployment
- [ ] Health checks passing
- [ ] Voice recognition working
- [ ] Database connectivity verified
- [ ] CORS configuration tested
- [ ] Performance monitoring enabled
- [ ] Backup procedures in place

## 🆘 Support and Maintenance

### Regular Maintenance Tasks
- Monitor application logs
- Update dependencies regularly
- Backup database
- Monitor performance metrics
- Review security updates

### Scaling Considerations
- Horizontal scaling with load balancers
- Database read replicas
- CDN for global distribution
- Caching layers (Redis)

---

**Need help with deployment? Check the troubleshooting section or create an issue in the repository.**

