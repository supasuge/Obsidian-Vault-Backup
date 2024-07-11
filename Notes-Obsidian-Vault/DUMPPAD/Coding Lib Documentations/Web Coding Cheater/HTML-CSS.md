## Table of Contents



![[Pasted image 20231023222525.png]]

```html
    


<div class="user-box">
  <div class="user-id">
    <img class="user-picture" src="https://fbcdn-profile-a.akamaihd.net/hprofile-ak-ash3/c34.34.432.432/s160x160/575695_10101114843671310_1323509961_n.jpg"/>
    <div class="user-name">
      DU7S
    </div>
    <div class="dropdown-arrow"></div>
    <div class="dropdown-menu">
      <ul>
        <li>Settings</li>
        <li>Sports</li>
        <li>Log Out</li>
      </ul>
    </div>
  </div>
</div>

[CSS]
```CSS
body {
  height: 100%;
  background: url(http://pickupleagues.com/images/backgrounds/CC_fence-basketball.jpg) no-repeat center center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  font-family: Verdana, Arial, sans-serif;
}

* {
  margin: 0;
  padding: 0;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}

.user-box {
  margin: 0 auto;
  width: 400px;
}

.user-id {
  height: 100px;
  width: 100%;
  background: #fff;
  position: relative;
  cursor: pointer;
}

.user-picture {
  height: 100%;
  float: left;
}

.user-name {
  height: 100%;
  width: 300px;
  font-size: 1.5em;
  line-height: 2.5em;
  padding: 20px;
  float: left;
}

.dropdown-arrow {
  height: 0;
  width: 0;
	border-left: 10px solid transparent;
	border-right: 10px solid transparent;
	border-top: 10px solid #333;
  position: relative;
  top: 45px;
  left: 350px;
}

.user-id:hover .dropdown-arrow {
  top: 35px;
	border-left: 10px solid transparent;
	border-right: 10px solid transparent;
	border-bottom: 10px solid #333;
  border-top: 10px solid transparent;
}

.dropdown-menu {
  list-style-type: none;
  display: none;
  background: #555; /* Non-rgba compatable browsers */
  background: rgba(33, 33, 33, 0.9);
}

.user-id:hover .dropdown-menu {
  display: block;
  position: absolute;
  top: 100%;
}

.dropdown-menu ul li {
  list-style-type: none;
  display: block;
  height: 100px;
  width: 400px;
  font-size: 1.5em;
  line-height: 2.5em;
  padding: 20px;
  color: #fff;
  border-bottom: 2px solid #333;
}

.dropdown-menu ul li:last-child {
  border-bottom: none;
}

.dropdown-menu ul li:hover {
  background: #333;
  color: #fff;
}
```

