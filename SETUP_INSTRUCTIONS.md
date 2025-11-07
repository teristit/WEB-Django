# Django Platformer Game - Complete Setup Instructions

## Overview
This is a Django-based web version of a 2D platformer game with multiplayer support via WebSockets.

## Prerequisites
- Python 3.8 or higher
- Redis server (for WebSocket support)
- Git

## Installation Steps

### 1. Clone the Repository
```bash
git clone https://github.com/teristit/WEB-Django.git
cd WEB-Django
```

### 2. Create Virtual Environment
```bash
python -m venv venv

# On Windows:
venv\Scripts\activate

# On macOS/Linux:
source venv/bin/activate
```

### 3. Install Dependencies
```bash
cd webdjango
pip install -r ../requirements.txt
```

**Note about Pillow:** If you encounter Pillow installation errors, it's commented out by default as it's only needed for sprite uploads. If you need it:
- On Windows: Install Visual C++ Build Tools
- On Linux: `sudo apt-get install python3-dev python3-pip libjpeg-dev zlib1g-dev`
- On macOS: `brew install libjpeg`

Then uncomment the Pillow line in requirements.txt and reinstall.

### 4. Install and Start Redis

**Windows:**
- Download Redis for Windows from: https://github.com/microsoftarchive/redis/releases
- Or use WSL2 with: `sudo apt-get install redis-server`
- Start: `redis-server`

**macOS:**
```bash
brew install redis
brew services start redis
```

**Linux:**
```bash
sudo apt-get install redis-server
sudo systemctl start redis
sudo systemctl enable redis
```

### 5. Configure Django Settings

Edit `webdjango/platformer_project/settings.py` if needed:
- Set `DEBUG = True` for development
- Update `ALLOWED_HOSTS` if deploying
- Configure Redis connection if using non-default settings

### 6. Run Database Migrations
```bash
cd webdjango
python manage.py makemigrations
python manage.py migrate
```

### 7. Create Superuser (Optional)
```bash
python manage.py createsuperuser
```

### 8. Collect Static Files (for production)
```bash
python manage.py collectstatic --noinput
```

### 9. Run Development Server

**Option A: Using Django Development Server (HTTP only)**
```bash
python manage.py runserver
```
Access at: http://127.0.0.1:8000/

**Option B: Using Daphne (HTTP + WebSockets)**
```bash
daphne -b 0.0.0.0 -p 8000 platformer_project.asgi:application
```
Access at: http://127.0.0.1:8000/

## Project Structure
```
WEB-Django/
├── requirements.txt           # Python dependencies
├── webdjango/                 # Django project root
│   ├── manage.py              # Django management script
│   ├── platformer_project/    # Main project settings
│   │   ├── settings.py        # Django settings
│   │   ├── urls.py            # URL routing
│   │   ├── asgi.py            # ASGI config (WebSockets)
│   │   └── wsgi.py            # WSGI config
│   ├── game/                  # Game application
│   │   ├── models.py          # Database models
│   │   ├── views.py           # View functions
│   │   ├── urls.py            # App URL patterns
│   │   ├── consumers.py       # WebSocket consumers
│   │   ├── routing.py         # WebSocket routing
│   │   ├── admin.py           # Admin interface
│   │   └── migrations/        # Database migrations
│   ├── templates/             # HTML templates
│   │   └── game/
│   │       └── game.html      # Main game template
│   └── static/                # Static files (CSS, JS, images)
│       ├── css/
│       ├── js/
│       └── images/
└── README.md
```

## Features
- ✅ Web-based 2D platformer
- ✅ User authentication and registration
- ✅ Save game progress to database
- ✅ Leaderboard system
- ✅ Achievement tracking
- ✅ WebSocket support for multiplayer (requires Redis)
- ✅ Admin interface for managing levels

## Accessing the Application

### Main Pages:
- **Home/Game:** http://127.0.0.1:8000/
- **Play:** http://127.0.0.1:8000/play/
- **Leaderboard:** http://127.0.0.1:8000/leaderboard/
- **Achievements:** http://127.0.0.1:8000/achievements/
- **Admin:** http://127.0.0.1:8000/admin/

### API Endpoints:
- `POST /api/register/` - User registration
- `POST /api/login/` - User login
- `GET /api/logout/` - User logout
- `POST /api/start/` - Start new game session
- `POST /api/update/` - Update game state
- `POST /api/end/` - End game session

## Troubleshooting

### Pillow Installation Errors
If you see "Failed to build 'Pillow'" errors:
1. Pillow is optional - the game works without it
2. It's only needed if you want to upload sprite images through admin
3. Follow platform-specific instructions above to install system dependencies

### Redis Connection Errors
If WebSockets don't work:
1. Verify Redis is running: `redis-cli ping` (should return "PONG")
2. Check Redis is on port 6379 or update settings.py
3. For development without multiplayer, you can disable channels_redis

### Static Files Not Loading
```bash
python manage.py collectstatic
```

### Database Errors
```bash
python manage.py makemigrations game
python manage.py migrate
```

### Import Errors
Make sure you're in the `webdjango` directory when running commands:
```bash
cd webdjango
python manage.py runserver
```

## Development vs Production

### Development (Current Setup)
- DEBUG = True
- SQLite database
- Django development server or Daphne
- Local Redis

### Production Recommendations
- Set DEBUG = False
- Use PostgreSQL or MySQL
- Use proper ASGI server (Daphne, Uvicorn)
- Use Redis cluster or managed service
- Set up proper SECRET_KEY
- Configure ALLOWED_HOSTS
- Use environment variables for sensitive data
- Set up HTTPS/SSL
- Use reverse proxy (Nginx)

## Next Steps

1. **Add Game Levels:** Use Django admin to create levels
2. **Add Sprites:** Upload game sprites through admin (requires Pillow)
3. **Customize Templates:** Edit templates in `webdjango/templates/game/`
4. **Add Game Logic:** Modify `game.html` JavaScript for gameplay
5. **Test Multiplayer:** Start Redis and test WebSocket connections

## Support

For issues and questions:
- Check the troubleshooting section above
- Review Django logs for errors
- Verify all services are running (Django, Redis)
- Check browser console for JavaScript errors

## License

See LICENSE file in repository.
