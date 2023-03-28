<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Control de entrada de invitados</title>
</head>
<body>
	<h1>Control de entrada de invitados</h1>
	<div id="qr-reader"></div>
	<div id="info"></div>
	<script src="https://rawgit.com/mebjas/html5-qrcode/master/html5-qrcode.min.js"></script>
	<script>
		// Crea un lector de códigos QR
		var reader = new Html5Qrcode('qr-reader');

		// Escanea un código QR y muestra la información del invitado
		reader.start({ facingMode: "environment" }, function (code) {
			fetch(`/invitados/${code}`)
				.then(response => response.json())
				.then(data => {
					document.getElementById('info').innerHTML = `Bienvenido, ${data.nombre}`;
				})
				.catch(error => {
					document.getElementById('info').innerHTML = 'Código QR no válido.';
				})
				.finally(() => {
					reader.stop();
				});
		}, function (error) {
