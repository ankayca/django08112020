1.)in setting add:
ALLOWED_HOSTS = ['127.0.0.1', '.pythonanywhere.com']
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
TIME_ZONE = 'Europe/Istanbul'

after that 
python manage.py migrate 
python manage.py runserver 

we create a new django model with:
python model.py startapp blogumbenim

add blogumbenim to settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blogumbenim',
]

to create a post model go to blogumbenim/models.py :
'''
from django.db import models
from django.utils import timezone

//Post name of our model (we can change it)
class Post(models.Model): //models.Model say that I am a django model!!
    author = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
            default=timezone.now)
    published_date = models.DateTimeField(
            blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
'''
models.CharField – this is how you define text with a limited number of characters.
models.TextField – this is for long text without a limit. Sounds ideal for blog post content, right?
models.DateTimeField – this is a date and time.
models.ForeignKey – this is a link to another model.

to create special model ,you can use https://docs.djangoproject.com/en/2.0/ref/models/fields/#field

to declare our model changes :
python manage.py makemigrations blogumbenim

after that :
python manage.py migrate blogumbenim

to show out new model in admin panel :
blogumbenmi/admin.py add :
'''
from django.contrib import admin
from .models import Post

admin.site.register(Post)

'''
add superuser as an admin:

python manage.py createsuperuser

create a git repository from github.com //dont check .gitignore ,README.md file when create a repository
'''
git init
git config --global user.name
git config --global user.email
'''
add .gitingnore to project folder with 

touch .gitignore

add this lines: 
'''
*.pyc
*~
__pycache__
myvenv
db.sqlite3
/static
.DS_Store
'''
after that :
git add --all .

Add this line to ankayca/urls.py : 
'''
from django.urls import path, include

path('', include('blogumbenim.urls')),
'''
create a new file urls.py from blogumbenim/urls.py
add
'''
from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
]
'''
add to blogumbenim/views.py: 

def post_list(request):
    return render(request, 'blog/post_list.html', {})
 
 add new html file to /blogumbenim/templates/blogumbenim/post_list.html :
 
 
 
