##Hello World
```html
<!DOCTYPE html>
<!-- ng-app �������б���������Ԫ�ض�����AngularJS Ӧ�� -->
<html ng-app>
	<head>
		<title>Simple app</title>
		<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.2.13/angular.js"></script>
	</head>
	<body>
		<input ng-model="name" type="text" placeholder="Your name">
		<h1>Hello {{ name | number:2 }}</h1>
	</body>
</html>
```