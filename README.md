# Voice Task Manager - Neurodiversity-Friendly Productivity App

A comprehensive voice-powered task management application designed specifically for neurodivergent users, featuring task breakdown, focus timing, and voice interaction capabilities.

## 🎯 Features

### Core Functionality

- **Voice-Powered Task Management**: Create and manage tasks using voice commands
- **Pomodoro Focus Timer**: 25-minute focus sessions with break intervals
- **Task Prioritization**: High, medium, and low priority system with visual indicators
- **Task Breakdown**: Automatic breakdown of large tasks into manageable subtasks
- **Real-time Voice Recognition**: Continuous listening for voice commands
- **Neurodiversity-Friendly Design**: Clean interface with high contrast and clear visual hierarchy

### Accessibility Features

- **ADHD Support**: Task breakdown and focus timing
- **Executive Function Support**: Structured workflow and priority system
- **Sensory Considerations**: Minimal visual clutter and consistent color coding
- **Voice Interaction**: Reduces cognitive load of complex navigation

## 🏗️ Architecture

### Backend (Flask API)

- **Framework**: Flask with SQLAlchemy ORM
- **Database**: SQLite (development) / PostgreSQL (production ready)
- **API**: RESTful endpoints for tasks, users, and focus sessions
- **CORS**: Configured for frontend integration

### Frontend (React Application)

- **Framework**: React 19 with Vite
- **UI Components**: Shadcn/UI with Tailwind CSS
- **Voice Integration**: Web Speech API
- **State Management**: React hooks and context

## 📁 Project Structure

```bash
voice-task-manager/
├── voice_task_manager_api/          # Backend Flask API
│   ├── src/
│   │   ├── main.py                  # Application entry point
│   │   ├── models/                  # Database models
│   │   │   ├── user.py
│   │   │   ├── task.py
│   │   │   └── focus_session.py
│   │   └── routes/                  # API endpoints
│   │       ├── user.py
│   │       ├── task.py
│   │       └── focus_session.py
│   ├── requirements.txt             # Python dependencies
│   └── venv/                        # Virtual environment
│
├── voice-task-manager-frontend/     # Frontend React App
│   ├── src/
│   │   ├── App.jsx                  # Main application component
│   │   ├── components/              # React components
│   │   │   ├── TaskList.jsx
│   │   │   ├── TaskForm.jsx
│   │   │   ├── FocusTimer.jsx
│   │   │   └── VoiceInterface.jsx
│   │   └── App.css                  # Styling
│   ├── package.json                 # Node.js dependencies
│   └── public/                      # Static assets
│
├── README.md                        # This file
├── DEPLOYMENT.md                    # Deployment instructions
├── research_report.md               # Design research and principles
└── testing_report.md                # Comprehensive testing results
```

## 🚀 Quick Start

### Prerequisites

- Python 3.11+
- Node.js 20+
- Git

### Backend Setup

1. **Clone and navigate to backend**:

   ```bash
   cd voice_task_manager_api
   ```

2. **Create virtual environment**:

   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**:

   ```bash
   pip install -r requirements.txt
   ```

4. **Run the backend**:

   ```bash
   python src/main.py
   ```

   Backend will be available at `http://localhost:5000`

### Frontend Setup

1. **Navigate to frontend**:

   ```bash
   cd voice-task-manager-frontend
   ```

2. **Install dependencies**:

   ```bash
   pnpm install  # or npm install
   ```

3. **Start development server**:

   ```bash
   pnpm run dev  # or npm run dev
   ```

   Frontend will be available at `http://localhost:5173`

## 🎤 Voice Commands

The application supports natural language voice commands:

- **Task Management**:
  - "Create task called [task name]"
  - "Add task [description] with high priority"
  - "Show my tasks"
  - "Break down [task name]"

- **Focus Sessions**:
  - "Start focus session"
  - "Take a break"
  - "Start short break"
  - "Start long break"

- **General**:
  - "Help"
  - "Stop listening"

## 🔧 API Endpoints

### Tasks

- `GET /api/tasks` - Retrieve all tasks
- `POST /api/tasks` - Create new task
- `PUT /api/tasks/{id}` - Update task
- `DELETE /api/tasks/{id}` - Delete task
- `POST /api/tasks/{id}/breakdown` - Break task into subtasks

### Focus Sessions

- `POST /api/focus-sessions/start` - Start focus session
- `GET /api/focus-sessions/active` - Get active session
- `POST /api/focus-sessions/{id}/pause` - Pause session
- `POST /api/focus-sessions/{id}/end` - End session

### Users

- `GET /api/users` - List users
- `POST /api/users` - Create user
- `GET /api/users/{id}` - Get user details

## 🎨 Design Principles

### Neurodiversity-Friendly Features

1. **Visual Clarity**: High contrast colors, clear typography, minimal clutter
2. **Consistent Patterns**: Predictable navigation and interaction patterns
3. **Cognitive Load Reduction**: Voice commands reduce need for complex navigation
4. **Time Management**: Built-in Pomodoro timer helps with focus and time awareness
5. **Task Breakdown**: Large tasks automatically broken into manageable pieces

### Color Coding System

- **Green**: Active states, success, go actions
- **Red**: Stop actions, high priority, alerts
- **Blue**: Information, secondary actions
- **Yellow**: Warnings, breaks, attention needed

## 🔒 Security Considerations

- Input validation on all API endpoints
- CORS properly configured for frontend integration
- SQL injection prevention through SQLAlchemy ORM
- Environment-based configuration for production deployment

## 📱 Browser Compatibility

- **Chrome/Edge**: Full support including voice recognition
- **Firefox**: Full support including voice recognition
- **Safari**: Full support including voice recognition
- **Mobile Browsers**: Responsive design with touch-friendly interface

## 🚀 Deployment Options

### Option 1: Local Development

Follow the Quick Start guide above for local development and testing.

### Option 2: Cloud Deployment

See `DEPLOYMENT.md` for detailed instructions on deploying to:

- Heroku
- Vercel
- DigitalOcean
- AWS

### Option 3: Docker Deployment

Docker configurations included for containerized deployment.

## 🧪 Testing

The application has been comprehensively tested:

- ✅ All API endpoints functional
- ✅ Voice recognition working across browsers
- ✅ Focus timer and Pomodoro technique validated
- ✅ Task management and prioritization tested
- ✅ Neurodiversity-friendly design principles validated

See `testing_report.md` for detailed testing results.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🆘 Support

For support, feature requests, or bug reports:

1. Check the testing report for known issues
2. Review the deployment guide for setup problems
3. Create an issue with detailed description and steps to reproduce

## 🙏 Acknowledgments

- Designed with input from neurodiversity research
- Built with accessibility-first principles
- Inspired by the Pomodoro Technique for focus management
- Voice recognition powered by Web Speech API

---

### **Built with ❤️ for the neurodivergent community**

### taskManager-VoiceAgent
