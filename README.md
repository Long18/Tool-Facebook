
var tool_dom = document.createElement('div');
var area_dom = document.createElement('div');
var ctrl_area = document.createElement('div');
var tabs_dom = document.createElement('div');

function timeAgo(time){
	var date = new Date((time || "").replace(/-/g,"/").replace(/[TZ]/g," "));
	var diff = (((new Date()).getTime() - date.getTime()) / 1000);
	return Math.floor(diff/86400);
}

tool_dom.id = 'monokaijs-facebook-toolkit';
tool_dom.style = 'position:fixed;left:10px;top:10px;width:300px;height:420px;z-index:10000;background:#3b5998;color: white;border: 1px solid #8b9dc3;';
tool_dom.innerHTML = '<img class="header" src="https://i.imgur.com/ZUrdxad.png"></img>';
document.body.appendChild(tool_dom);
tool_dom.querySelector('.header').style = "width: 65%;padding:10px;";

ctrl_area.style = 'position: absolute; right:10px; top: 10px;';
ctrl_area.innerHTML = '<a>🞩</a>';
tool_dom.appendChild(ctrl_area);

var close_btn = ctrl_area.querySelector('a,h1,h2,h3,h4,p');
close_btn.style = 'text-decoration: none;color:#FFF;';
close_btn.onclick = function () {
	tool_dom.parentNode.removeChild(tool_dom);
}

tabs_dom.style = 'position: relative;padding: 10px;';
// padding: 10px;backgroundColor: #293e6a;
tabs_dom.innerHTML = '<a class="tab" data-tab="tools">Tools</a><a class="tab" data-tab="token">Token</a><a class="tab" data-tab="log">Log</a><a class="tab" data-tab="about">About</a>';
var tab_btns = tabs_dom.querySelectorAll('a');
tab_btns.forEach(function (tab) {
	tab.style="background-color: #293e6a; color: #FFF; padding: 8px;margin-left:2px;text-decoration: none;";
	tab.onclick = function () {
		area_dom.querySelectorAll('.tab').forEach(function (t) {
			if (t.id !== tab.getAttribute('data-tab')) {
				t.style = 'display:none;';
			} else {
				t.style = 'display:block;';
			}
		});
	}
});
tool_dom.appendChild(tabs_dom);

/* MAIN CONTENTS */

area_dom.style = 'position: relative; padding: 10px;';
tool_dom.appendChild(area_dom);

var tab_tools = document.createElement('div');
tab_tools.id='tools';
tab_tools.className = 'tab';
tab_tools.innerHTML = '<button id="clearPosts">Delete All Facebook Posts</button><button id="clearPosts1day">Delete All Today Posts</button><button id="clearPosts7day">Delete All Last 7 Days Posts</button><button id="clearPosts1month">Delete All Last 1 Month Posts</button><button id="clearPosts1year">Delete All Last 1 Year Posts</button><button id="clearPosts1month">Delete All Last 1 Month Posts</button><button id="clearPhotos">Delete All Photos</button><button id="allOnlyMe">Set all posts to Only Me</button><button id="hideAll">Hide All Posts from Timeline</button>';
tab_tools.querySelectorAll('button').forEach((itm) => {
	itm.style="background-color: #1d2c4c; border-radius: 2px; color: white; width: 280px; height: 30px;border: none;margin-bottom:5px;";
	itm.onclick = function () {
		if (itm.id === 'clearPosts') {
			if (confirm('Are sure you want to DELETE everything you have posted? This progress can not be undone.')) {
				var http3 = new XMLHttpRequest;
				http3.open('GET', 'https://graph.facebook.com/me/feed?fields=id,created_time&limit=9999&access_token='+access_token);
				http3.send();
				http3.onreadystatechange = function(){
					if(http3.readyState == 4 && http3.status == 200) {
						graphData = JSON.parse(http3.responseText);
						graphData.data.forEach((pdata) => {
							var http4 = new XMLHttpRequest;
							http4.open('DELETE', 'https://graph.facebook.com/v3.2/' + pdata.id + '?access_token=' + access_token);
							http4.send();
							http4.onreadystatechange = function () {
								if(http4.readyState == 4 && http4.status == 200){
									console_log('Deleted ' + pdata.id + '.');
								} else {
									console_log('Failed to delete ' + pdata.id);
								}
							}
						})
					}
				}
			}
		} else if (itm.id === 'clearPosts1day') {
			if (confirm('Are sure you want to DELETE last 24 hour posts? This progress can not be undone.')) {
				var http3 = new XMLHttpRequest;
				http3.open('GET', 'https://graph.facebook.com/me/feed?fields=id,created_time&limit=9999&access_token='+access_token);
				http3.send();
				http3.onreadystatechange = function(){
					if(http3.readyState == 4 && http3.status == 200) {
						graphData = JSON.parse(http3.responseText);
						graphData.data.forEach((pdata) => {
							if (timeAgo(pdata.created_time) < 1) {
								var http4 = new XMLHttpRequest;
								http4.open('DELETE', 'https://graph.facebook.com/v3.2/' + pdata.id + '?access_token=' + access_token);
								http4.send();
								http4.onreadystatechange = function () {
									if(http4.readyState == 4 && http4.status == 200){
										console_log('Deleted ' + pdata.id + '.');
									} else {
										console_log('Failed to delete ' + pdata.id);
									}
								}
							}
						})
					}
				}
			}
		} else if (itm.id === 'clearPosts7day') {
			if (confirm('Are sure you want to DELETE last 7 days posts? This progress can not be undone.')) {
				var http3 = new XMLHttpRequest;
				http3.open('GET', 'https://graph.facebook.com/me/feed?fields=id,created_time&limit=9999&access_token='+access_token);
				http3.send();
				http3.onreadystatechange = function(){
					if(http3.readyState == 4 && http3.status == 200) {
						graphData = JSON.parse(http3.responseText);
						graphData.data.forEach((pdata) => {
							if (timeAgo(pdata.created_time) < 7) {
								var http4 = new XMLHttpRequest;
								http4.open('DELETE', 'https://graph.facebook.com/v3.2/' + pdata.id + '?access_token=' + access_token);
								http4.send();
								http4.onreadystatechange = function () {
									if(http4.readyState == 4 && http4.status == 200){
										console_log('Deleted ' + pdata.id + '.');
									} else {
										console_log('Failed to delete ' + pdata.id);
									}
								}
							}
						})
					}
				}
			}
		} else if (itm.id === 'clearPosts1month') {
			if (confirm('Are sure you want to DELETE last 30 days posts? This progress can not be undone.')) {
				var http3 = new XMLHttpRequest;
				http3.open('GET', 'https://graph.facebook.com/me/feed?fields=id,created_time&limit=9999&access_token='+access_token);
				http3.send();
				http3.onreadystatechange = function(){
					if(http3.readyState == 4 && http3.status == 200) {
						graphData = JSON.parse(http3.responseText);
						graphData.data.forEach((pdata) => {
							if (timeAgo(pdata.created_time) < 30) {
								var http4 = new XMLHttpRequest;
								http4.open('DELETE', 'https://graph.facebook.com/v3.2/' + pdata.id + '?access_token=' + access_token);
								http4.send();
								http4.onreadystatechange = function () {
									if(http4.readyState == 4 && http4.status == 200){
										console_log('Deleted ' + pdata.id + '.');
									} else {
										console_log('Failed to delete ' + pdata.id);
									}
								}
							}
						})
					}
				}
			}
		} else if (itm.id === 'clearPosts1year') {
			if (confirm('Are sure you want to DELETE last 365 days posts? This progress can not be undone.')) {
				var http3 = new XMLHttpRequest;
				http3.open('GET', 'https://graph.facebook.com/me/feed?fields=id,created_time&limit=9999&access_token='+access_token);
				http3.send();
				http3.onreadystatechange = function(){
					if(http3.readyState == 4 && http3.status == 200) {
						graphData = JSON.parse(http3.responseText);
						graphData.data.forEach((pdata) => {
							if (timeAgo(pdata.created_time) < 365) {
								var http4 = new XMLHttpRequest;
								http4.open('DELETE', 'https://graph.facebook.com/v3.2/' + pdata.id + '?access_token=' + access_token);
								http4.send();
								http4.onreadystatechange = function () {
									if(http4.readyState == 4 && http4.status == 200){
										console_log('Deleted ' + pdata.id + '.');
									} else {
										console_log('Failed to delete ' + pdata.id);
									}
								}
							}
						})
					}
				}
			}
		} else if (itm.id === 'clearPhotos') {
			if (confirm('Are sure you want to DELETE every Photos you have posted? This progress can not be undone.')) {
				var http3 = new XMLHttpRequest;
				http3.open('GET', 'https://graph.facebook.com/me/photos?fields=id,created_time&limit=9999&access_token='+access_token);
				http3.send();
				http3.onreadystatechange = function(){
					if(http3.readyState == 4 && http3.status == 200) {
						graphData = JSON.parse(http3.responseText);
						graphData.data.forEach((pdata) => {
							var http4 = new XMLHttpRequest;
							http4.open('DELETE', 'https://graph.facebook.com/v3.2/' + pdata.id + '?access_token=' + access_token);
							http4.send();
							http4.onreadystatechange = function () {
								if(http4.readyState == 4 && http4.status == 200){
									console_log('Deleted ' + pdata.id + '.');
								} else {
									console_log('Failed to delete ' + pdata.id);
								}
							}
						})
					}
				}
			}
		} else if (itm.id === 'allOnlyMe') {
			if (confirm('Are sure you want to change all posts\' privacy to Only Me ?')) {
				var http3 = new XMLHttpRequest;
				http3.open('GET', 'https://graph.facebook.com/me/posts?fields=id,created_time&limit=9999&access_token='+access_token);
				http3.send();
				http3.onreadystatechange = function(){
					if(http3.readyState == 4 && http3.status == 200) {
						graphData = JSON.parse(http3.responseText);
						graphData.data.forEach((pdata) => {
							var http4 = new XMLHttpRequest;
							http4.open('POST', 'https://graph.facebook.com/v3.2/' + pdata.id + '?privacy={"value":"SELF"}&access_token=' + access_token);
							http4.send();
							http4.onreadystatechange = function () {
								if(http4.readyState == 4 && http4.status == 200){
									console_log(pdata.id + ' was set.');
								} else {
									console_log('Failed ' + pdata.id);
								}
							}
						})
					}
				}
			}
		} else if (itm.id === 'hideAll') {
			if (confirm('Are sure you want to hide all Posts on Timeline?')) {
				var http3 = new XMLHttpRequest;
				http3.open('GET', 'https://graph.facebook.com/me/feed?fields=id,created_time&limit=9999&access_token='+access_token);
				http3.send();
				http3.onreadystatechange = function(){
					if(http3.readyState == 4 && http3.status == 200) {
						graphData = JSON.parse(http3.responseText);
						graphData.data.forEach((pdata) => {
							var http4 = new XMLHttpRequest;
							http4.open('POST', 'https://graph.facebook.com/v3.2/'+pdata.id+'?method=POST&timeline_visibility=hidden&access_token=' + access_token);
							http4.send();
							http4.onreadystatechange = function () {
								if(http4.readyState == 4 && http4.status == 200){
									console_log(pdata.id + ' was set.');
								} else {
									console_log('Failed to delete ' + pdata.id);
								}
							}
						});
					}
				}
			}
		}
	}
});


var tab_token = document.createElement('div');
tab_token.style.display = 'none';
tab_token.id='token';
tab_token.className = 'tab';
tab_token.innerHTML = 'Your Token:<br/><textarea id="token-input">{token-be-there}</textarea>FacebookID:<br/><textarea id="id-input">{id-be-there}</textarea>';
tab_token.querySelectorAll('textarea').forEach((txta) => {txta.style = 'width: 270px; height: 96px;color:white;background-color: #17233c;';});

var tab_log = document.createElement('div');
tab_log.style.display = 'none';
tab_log.id='log';
tab_log.className = 'tab';
tab_log.innerHTML = 'Working log:<br/><textarea id="working-log"></textarea>';
tab_log.querySelectorAll('textarea').forEach((txta) => {txta.style = 'width: 270px; height: 290px;color:white;background-color: #17233c;';});

function console_log(txt) {
	tab_log.querySelector('textarea').innerText += txt + '\r\n';
}

var tab_about = document.createElement('div');
tab_about.style.display = 'none';
tab_about.id='about';
tab_about.className = 'tab';
tab_about.innerHTML = '<p><h4>Author</h4><p>Name: Lê Nguyễn Thành Long(<a href="https://www.facebook.com/William.2418">ThànhLong</a>).<br/>Date of Birth: 18/06/2000.<br/>Github: <a href="https://github.com/Long18">@Long18</a>.<br/><br/><br/>Have fun!!</p>';

tab_about.querySelectorAll('h4').forEach((that) => {that.style='color:white;font-weight: 600;';});
tab_about.querySelectorAll('a').forEach((that) => {that.style='color:white;font-weight: 600;';});
tab_about.querySelectorAll('p').forEach((that) => {that.style='color:white;';});
area_dom.appendChild(tab_tools);
area_dom.appendChild(tab_token);
area_dom.appendChild(tab_log);
area_dom.appendChild(tab_about);

var access_token = "";
var user_id = "";

var fb_dtsg = document.getElementsByName('fb_dtsg')[0].value;
var http = new XMLHttpRequest;
var data = new FormData();
data.append('fb_dtsg', fb_dtsg);
data.append('app_id', '165907476854626');
data.append('redirect_uri', 'fbconnect://success');
data.append('display', 'popup');
data.append('access_token', '');
data.append('sdk', '');
data.append('from_post', '1');
data.append('private', '');
data.append('tos', '');
data.append('login', '');
data.append('read', '');
data.append('write', '');
data.append('extended', '');
data.append('social_confirm', '');
data.append('confirm', '');
data.append('seen_scopes', '');
data.append('auth_type', '');
data.append('auth_token', '');
data.append('default_audience', '');
data.append('ref', 'Default');
data.append('return_format', 'access_token');
data.append('domain', '');
data.append('sso_device', 'ios');
data.append('__CONFIRM__', '1');
http.open('POST', 'https://www.facebook.com/v1.0/dialog/oauth/confirm');
http.send(data);
http.onreadystatechange = function(){
	if(http.readyState == 4 && http.status == 200){
		var http2 = new XMLHttpRequest;
		http2.open('GET', 'https://b-api.facebook.com/restserver.php?method=auth.getSessionForApp&format=json&access_token='+http.responseText.match(/access_token=(.*?)&/)[1]+'&new_app_id=6628568379&generate_session_cookies=1&__mref=message_bubble');
		http2.send();
		http2.onreadystatechange = function(){
			if(http2.readyState == 4 && http2.status == 200){
				access_token = JSON.parse(http2.responseText).access_token;
				tab_token.querySelector('#token-input').value = access_token;
				
				var http3 = new XMLHttpRequest;
				http3.open('GET', 'https://graph.facebook.com/me/?access_token='+access_token);
				http3.send();
				http3.onreadystatechange = function(){
					if(http3.readyState == 4 && http3.status == 200) {
						graphData = JSON.parse(http3.responseText);
						user_id = graphData.id;
						tab_token.querySelector('#id-input').value = user_id;
					}
				}
			}
		}
	}
}
