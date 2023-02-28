workok superlists37

me muevo a la carpeta de TDDpy

corro `python functional_test.py` tira erra

Suponiendo que tengo Django y Selenium instalado, creo un proyecto con el comando:
`django-admin.py startproject superlists

Me paro en la carpeta 'superlists' mas exterior y corro:
`python manage.py runserver`

Ahi tira un warning en colorado y tengo el servidor levantado. 

Abro otra termiunal, abro el virtualenv, y vuelvo a correr los tests.

Funciona!

# Git

Add files to .gitignore:

```bash
$ echo <"file_name"> >> .gitignore
```

To remove unwanted tracked files run:

```bash
$ git rm -r --cached <file_name>
```