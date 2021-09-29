# Flask

Dokumentation: https://flask.palletsprojects.com/en/2.0.x/

Github-repo: https://github.com/AlexisFlach/flask-presentation

Ett väldigt populärt framework i Python är Flask. Med framework menas en grupp med resurser och verktyg i syfte att skapa webbapplikationer och hemsidor. Det förenklar helt enkelt vårt arbete då det finns en struktur i hur vi ska bygga våra applikationer. Vi får ett redan färdigt system att förhålla oss till vilket ger oss en tydlig och effektiv struktur. 

#### Skapa en simpel Flask Applikation

Skapa en mapp "flaskapp". 

**1. Skapa en virtuell miljö** 

Vi skapar en virtuell miljö i vårt projekt för att hantera dess dependencies. En dependency är en essentiell del av vår applikation som måste finnas med för att applikationen ska fungera.

På Mac:

```
python3 -m venv venv
```

På Windows

```
py -3 -m venv venv
```

**2. Aktivera den virtuella miljön**

På Mac:

```
. venv/bin/activate
```

På Windows:

```
venv\Scripts\activate
```

För att inaktivera

```
deactivate
```

**3. Installera Flask**

```
pip install Flask
```

**4. Skapa entrypoint för applikationen: app.py**

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def index():
    return "<h1>Hello World!</h1>"
```

1. Vi börjar med att importera class Flask.

2. Vi skapar en instans av class Flask. Vår applikationen finns nu lagrad i variabeln **app**.

   **__name__** står i princip för namnet på filen.

3. Vi specificierar en url route, i detta fall till "/" genom att en såkallad decorator. När vi får in en request till den url:en vet Flask vilken funktion som ska triggas igång.

4. I detta fall returnar funktionen ett HTML-element med texten "Hello World".

**5** Kör applikationen

Öppna en terminal och kör

```
flask run
```

**6 sätt applikationen till development mode**

Gå in i terminalen och kör:

```
export FLASK_ENV=development
```

```
flask run
```

**7. Annat sätt vi kan starta vår applikation**

Förutom att köra flask run kan vi istället på slutet av vår app.py-fil lägga till.

```
if __name__ == '__main__':
  app.run(debug=True)
```

och i terminalen köra

```
python3 app.py
```

**8. Skapa requirements.txt**

Vi kan lagra våra dependencies i en requirements.txt-fil genom att köra

```
pip freeze > requirements.txt
```

#### Render Templates

Flask är som tidigare har nämts är ett library med funktioner vi kan använda oss av för att förenkla vår tillvaro. render_templates är en sådan. render_templates letar efter templates att rendera, hämtar den, så att vi kan displaye dess kontent i webbläsaren.

**1. app.py**

```
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def index():
    return render_template("index.html")
```

**2. Skapa en ny mapp "templates"**

**filstruktur**

```
flaskapp
	venv
	app.py
	templates/
		index.html
```

**3. index.html**

```
<h1>Hello World</h1>
```

**4.  Kör applikationen igen**

```
flask run
```

**5. About Page**

```
@app.route("/about")
def about():
  return render_template("about.html")
```

**6. template för about**

**filstruktur**

```
flaskapp
	venv
	app.py
	templates/
		index.html
		about.html
```

**about.html**

```
<h2>About Page</h2>
```

#### Forms

Ytterligare en funktion, eller i detta fall, objekt, som vi får gratis genom att använda Flask är **request**. Vad request gör är att hanterar inkommande requests och ger en tillgång till det som skickats med. 

**1. Inkludera request i applikationen**

```
from flask import Flask, render_template, request
```

**2. Skapa ett formulär i  index.html**

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <form action="/register" method="post">
    <input name="name" type="text">
    <input type="submit">
  </form>
</body>
</html>
```

Lägg märke till action="/register" method="post".

Vi måste nu hantera en post request i till route /register.

**3. Lägg till POST request i app.py**

```
@app.route("/register", methods=["POST"])
def register():
  return render_template('home.html', name=request.form.get("name"))
```

**4. home.html**

```
<h2>Welcome, {{name}}</h2>
```

#### Layout med Jinja

Flask kommer med en template engine som heter Jinja. Det är vad som möjliggjorde {{name}} i föregående exempel.

Vi kan effektivisera vår struktur med hjälp av jinja.

**1. Skapa layout.html**

```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Example Website</title>
</head>

<body>
  {% block content %}
  {% endblock content %}
</body>

</html>
```

Vi har nu grunden för vår layout och kan passa in övriga templates mellan blocken.

**2. home.html**

```
{% extends "layout.html" %}

{% block content %}

<h2>Welcome, {{name}}</h2>

{% endblock content%}
```

**3. about.html**

```
{% extends "layout.html" %}

{% block content %}

<h2>About Page</h2>

{% endblock content%}
```

