# Developing

1. **cd ~/Desktop**
2. **mkdir pages**
3. **cd pages**
4. **pipenv install django==3.0.3**
5. **pipenv shell**
6. **django-admin startproject pages_project .**
7. **python manage.py startapp pages**
8. Inside the *pages_project* folder open *settings.py* file and add under the INSTALLED_APPS **'pages.apps.PagesConfig',**
9. **python manage.py runserver** to check if connection has been established **ctrl+c** to exit runserver.
10. **mkdir templates**
11. **touch templates/home.html**
12. Inside the *pages_project* folder open *settings.py* file and add under the TEMPLATES change **'DIRS': []** to **'DIRS': [os.path.join(BASE_DIR, 'templates')]**
13. Inside the *pages* folder open *view.py* file and add under the *# Create your views here.* add:

        from django.views.generic import TemplateView

        class HomePageView(TemplateView):
            template_name = 'home.html'

14. Inside the *pages_project* folder open *urls.py* file and add **include** after the **path,**. Following, inside the **urlpatterns** add **path('', include('pages.urls')),**
15. **touch pages/urls.py**
16. Inside the *pages* folder open *urls.py* file and add:

        from django.urls import path
        from .views import HomePageView

        urlpatterns = [
            path('', HomePageView.as_view(), name='home'),
        ]

17. touch templates/about.html
18. Inside the *pages* folder open *view.py* file and add under the *HomePageView* Class add:

        class AboutPageView(TemplateView):
            template_name = 'about.html'

19. Inside the *pages* folder open *urls.py* file and add:

        from django.urls import path
        from .views import HomePageView, AboutPageView # new code

        urlpatterns = [
            path('', HomePageView.as_view(), name='home'),
            path('about/', AboutPageView.as_view(), name='about'), # new line of code
        ]
    No duplicates with previous code.
20. start **python manage.py runserver** to see if it works, if it does **ctrl+c** to exit
21. **touch templates/base.html**
22. Inside the *templates* folder open *base.html* file and add:

        <header>
            <a href="{% url 'home' %}">Home</a> | <a href="{% url 'about' %}">About</a>
        </header>

        {% block content %}
        {% endblock content %}

23. Inside the *templates* folder open *home.html* and *about.html* file and add:

    for home.html

        {% extends 'base.html' %}

        {% block content %}
        <h1>Homepage</h1>
        {% endblock content %}

    for about.html

        {% extends 'base.html' %}

        {% block content %}
        <h1>About page</h1>
        {% endblock content %}

24. start **python manage.py runserver** to see your code working.


# TESTING

1. Inside *pages* folder open *tests.py* file and add:

        from django.test import SimpleTestCase

        class SimpleTests(SimpleTestCase):
            def test_home_page_status_code(self):
                response = self.client.get('/')
                self.assertEqual(response.status_code, 200)

            def test_about_page_status_code(self):
                response = self.client.get('/about/')
                self.assertEqual(response.status_code, 200)

2. To start a test start **python manage.py test**

# Push to Github

1. git init
2. git add .
3. git commit -m"initial commit"
4. 

# Add to Heroku

1. heroku login
2. Inside *pipfile* file check *python_version* for appropriate version
3. pipenv lock
4. touch Procfile
5. Inside Procfile add:

        web: gunicorn pages_project.wsgi --log-file -
    **Gunicorn** is a *production* server while **Django** own server is a *development* server
6. pipenv install gunicorn==20.0.4
7. Inside *pages_project/settings.py* change one-line code **ALLOWED_HOSTS = []** to **ALLOWED_HOSTS = ['*']** this is a security measure against HTTP Host header attacks, * means that all domains are acceptable.
8. check **git status** for changes we made