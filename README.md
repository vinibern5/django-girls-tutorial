>  Um breve resumo do que aprendi seguindo o super tutorial do [djangogirls](https://tutorial.djangogirls.org)

# Instalação e inicialização do projeto

1. Criar virtual env
2. Ativar virtualenv
3. Criar requirements.txt com a versão desejada do django: `Django~=2.0.6` 
4. Instalar django a partir do requirements.txt
5. Iniciar o projeto a partir do django-admin usando o comando: `django-admin startproject mysite .`

6. Dentro da pasta mysite
   - Modificar a TIME_ZONE em settings.py p/ nossa timezone, no caso `'America/Sao_Paulo'`
   - Modificar a LANGUAGE_CODE em settings.py p/ codigo da nossa lingua, no caso `'pt-BR'`
   - Adicionar a URL para arquivos estaticos
   
   ```python
   STATIC_URL = '/static/'
   STATIC_ROOT = os.path.join(BASE_DIR, 'static')
   ```
   - Modificar a variável ALLOWED_HOSTS para `ALLOWED_HOSTS = ['127.0.0.1', '.pythonanywhere.com']` **PS**: Só é necessario por o `.pythonanywhere.com` porque esse código vai ser hospedado no pythonanywhere, se fosse outro host, seria algo diferente
7. Realizar as migrações do banco de dados rodando o comando `python manage.py migrate` na raiz do repositório
8. Rodar o servidor com `python manage.py runserver`

# Criando nosso primeiro model

1. Criar nossa aplicação, com o comando `python manage.py startapp blog` na raiz do repositório
2. Adicionar a aplicação *blog* na lista de aplicações, em settings.py do *mysite*. Adicionar 'blog' na lista INSTALLED_APPS
3. Criar o modelo no arquivo 'blog/models.py' (que é onde ficam os models da aplicação)
4. Criar os scripts para adicionar os modelos no banco de dados usando o comando `python manage.py makemigrations blog`
5. Aplique as migrações com o comando `python manage.py migrate blog`

# Criando novas views

1. Primeiro, em *mysite/urls.py*, devemos adicionar uma "referencia" ao arquivo de urls da nossa app (blog) na lista de urlpatterns.

    ```python
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('blog.urls')),
    ]
    ```
2. No diretório da nossa aplicação, que no nosso caso está em *blog/*, criamos um arquivo chamado *urls.py* para conter as urls da nossa aplicação. Esse arquivo vai conter uma lista de urlpatterns que serão utilizadas para identificar o que fazer para cada caminho de rota.
3. A lista de urlpatterns contem elementos path(), que são utilizados pelo django para resolver que view será usada em cada rota.
4. Nos criamos nossas views da nossa app (blog) em *blog/views.py*. As views são funções que recebem uma request e retornam algo p/ ser renderizado ou mostrado naquela view.

Exemplo de urlpatterns:
```python
urlpatterns = [
    path('', views.post_list, name='post_list'),
    path('post/<int:pk>', views.post_detail, name='post_detail'),
    path('post/new', views.post_new, name='post_new'),
    path('post/<int:pk>/edit/', views.post_edit, name='post_edit'),
]
```

Exemplo de view:
```python
def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/post_list.html', {'posts': posts})
```

# Templates

1. Os templates da nossa app (blog) devem ser salvos na pasta blog/templates/blog/
2. Para renderizar os templates, basta passar o nome do template nas funções de view
`render(request, 'blog/post_list.html', {'posts': posts})` 

# ORM

O django possui um ORM bem poderoso, para isso, basta importarmos o nosso model, e usar alguns metodos que o django põe no nosso model, muito pratico!
Também tem algumas funções embutidas que permitem obter instancias dos nossos dados, como get_object_or_404.

# Arquivos estaticos

Para nossa app (blog), os arquivos estaticos devem ir na pasta blog/static
Para usar os arquivos estaticos, devemos por a diretiva `{% load static %}` em nossos templates, e então importar os arquivos estaticos de forma similar a `<link rel="stylesheet" href="{% static 'css/blog.css' %}">`

# Forms do Django

O django já oferece alguns mecanismo para lidarmos com forms, resumidamente, é possível criarmos forms a partir do nosso model, e também obter uma instancia do nosso model a partir de um form, utilizando a classe ModelForm do django. Mais detalhes na [documentação do django.](https://docs.djangoproject.com/en/2.1/topics/forms/) 