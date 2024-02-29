# Simulacro Solucionado
1. El modelo de las impresoras de tipo láser.

for $impresora in doc("impresoras.xml")//impresora[@tipo = "láser"]
return $impresora

2. La marca y modelo (separados por un espacio en blanco) de las impresoras con más de un tamaño.

for $a in doc("impresoras.xml")//impresora[count(tamano) > 1]
return concat($a/marca/text(), " " , $a/modelo/text())

3. La marca y modelo (separados por un espacio en blanco) de las impresoras con tamaño A3 (pueden tener otros).

for $a in doc("impresoras.xml")//impresora
where $a/tamano = "A3"
return concat($a/marca/text(), " " , $a/modelo/text())

4. La marca y modelo (separados por un espacio en blanco) de las impresoras con tamaño A3 como único tamaño.

for $a in doc("impresoras.xml")//impresora[count (tamano) = 1]
where $a/tamano = "A3"
return concat($a/marca/text(), " " , $a/modelo/text())

5. El modelo de las impresoras en red.

for $a in doc("impresoras.xml")//impresora[enred]
return $a/modelo

6. La cantidad de impresoras guardadas en el fichero XML.

count(doc("impresoras.xml")//impresora)



7. Las impresoras (elementos <impresora>) compradas en 2018 o después. Los resultados se deben ordenar por año de compra (orden ascendente).
for $a in doc ("impresoras.xml")/impresoras/impresora[@compra >= 2018]
order by $a/@compra ascending
return $a

8. Las impresoras (elementos <impresora>) con un peso igual o superior a 5 kg.
for $a in doc ("impresoras.xml")/impresoras/impresora
where $a/peso >= 5
return $a


Si quiero solo texto:
for $a in doc ("impresoras.xml")/impresoras/impresora
where $a/peso >= 5
return concat ($a/modelo, " ", $a/marca)

9. Las impresoras (elementos <impresora>) que tienen cartucho con código C-456P.

for $a in doc ("impresoras.xml")/impresoras/impresora
where $a/cartucho = "C-456P"
return $a

10. La impresora (elemento <impresora>) más pesada.

Forma complicada:
let $a := max(doc("impresoras.xml")/impresoras/impresora/peso)
return doc("impresoras.xml")/impresoras/impresora[peso = $a]

Forma simplificada:

doc("impresoras.xml")/impresoras/impresora[peso = max(/impresoras/impresora/peso)]


Ejercicio extra
EXTRA. Una pregunta del examen será pasar el XML a formato HTML. Por ejemplo, ¿sabrías crear una tabla (o lista) con el número de serie, marca y modelo de las impresoras? 
Pare crear una lista:
<html>
<head>
  <title>Lista de Impresoras</title>
</head>
<body>
  <ul>
  {
    for $impresora in doc("impresoras.xml")//impresora
    return
    <li>
      <strong>Número de Serie:</strong> {$impresora/@numSerie}<br/>
      <strong>Marca:</strong> {$impresora/marca}<br/>
      <strong>Modelo:</strong> {$impresora/modelo}<br/>
    </li>
  }
  </ul>
</body>
</html>

Para crear una tabla:
<html>
<head>
  <title>Impresoras</title>
</head>
<body>
  <h1>Lista de Impresoras</h1>
  <table border="1">
    <tr>
      <th>Número de Serie</th>
      <th>Marca</th>
      <th>Modelo</th>
    </tr>
    {
      for $impresora in doc("impresoras.xml")//impresora
      return
        <tr>
          <td>{string($impresora/@numSerie)}</td>
          <td>{$impresora/marca}</td>
          <td>{$impresora/modelo}</td>
        </tr>
    }
  </table>
</body>
</html>


