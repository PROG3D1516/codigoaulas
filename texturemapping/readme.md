#Exercício:

Script que a partir de uma geometria personalizada faz o mapeamento de uma textura.

Quando se definem geometrias, podem (e devem) ser definidos os mapeamentos das texturas nas faces.

Neste exercício será definido um mapeamento de uma textura de dum dado para um cubo  definido "manualmente".



![alt text](https://github.com/PROG3D1516/codigoaulas/blob/master/textures/dado.png  "Textura a mapear num cubo")


##versão 0: mapeamento de textura num cubo
###Dados.

cubo de lado 1

lado da base inferior: 1 unidades
lado da base superior: 1 unidades
altura : 1 unidades

centrado na origem do referencial

A textura 'dados.png' que se encontra na pasta textures

###passo 1:
####Criar a geometria.

```javascript
var prisma = new THREE.Geometry();
```

###passo 2:
####Definir os vertices e adicioná-los à geometria.

Cada vértice será uma instância de um obecto do tipo Vector3.

```javascript
var v0 = new Vector3(-0.5,-0.5,+0.5); // vertice da base inferior
var v1 = new Vector3(+0.5,-0.5,+0.5); // vertice da base inferior
//... outros vertices
```

Os vértices são guardados num array "vertices".
A ordem pela qual são inseridos será importante para definir as faces.

```javascript
prisma.vertices.push(v0);
prisma.vertices.push(v1);
prisma.vertices.push(v2);

//... restantes vértices

prisma.vertices.push(v7);
```

###passo 3:
####Criar as faces.

A principal primitiva para criar as faces é o triângulo (Face3)
Cada face é definida por três indices do vector "vertices".
A ordem pela qual estes indices são carregados definem a  normal por defeito da face. (i.e. ordem no sentido horário, normal para "lá", regra no sentido anti-horário, normal para "cá").

```javascript
prisma.faces.push( new THREE.Face3(0,1,5) );
prisma.faces.push( new THREE.Face3(0,5,4) );

//... restantes faces
```

###passo 4: 
####Fazer o mapeamento da textura.

notar: as coordenadas da imagem/textura estão definidas no sistema normalizado UV

As Coordenadas podem ser armazenadas num THREE.Vector2(u,v)

Estes objetos não podem ser reutilizados na definição dos mapeamento. Caso haja pontos com mesmas coordenadas, deverão ser feitos clones, caso se queiram reutilizar .

A coordenada (u,v) = (0,0) fica no canto inferior esquerdo.

O mapeamento é feito fazendo associar a cada vértice de cada face uma coordenada de textura.

(ver imagem de textura dado.png)
Na imagem podemos identificar alguns pontos característicos
que resultam de uma definição de uma grelha 3 linhas x 4 colunas

```javascript
u000v000 = new THREE.UV(0.00,0.33);
u000v033 = new THREE.UV(0.00,0.33);
u000v066 = new THREE.UV(0.00,0.66);
u000v100 = new THREE.UV(0.00,1.00);

u025v000 = new THREE.UV(0.25,0.00);
u025v033 = new THREE.UV(0.25,0.33);
u025v066 = new THREE.UV(0.25,0.66);
u025v100 = new THREE.UV(0.25,1.00);

u050v000 = new THREE.UV(0.50,0.00);
u050v033 = new THREE.UV(0.50,0.33);
u050v066 = new THREE.UV(0.50,0.66);
u050v100 = new THREE.UV(0.50,1.00);

u075v000 = new THREE.UV(0.75,0.00);
u075v033 = new THREE.UV(0.75,0.33);
u075v066 = new THREE.UV(0.75,0.66);
u075v100 = new THREE.UV(0.75,1.00);

u100v000 = new THREE.UV(1.00,0.00);
u100v033 = new THREE.UV(1.00,0.33);
u100v066 = new THREE.UV(1.00,0.66);
u100v100 = new THREE.UV(1.00,1.00);
```

Assim e para o dado, supondo que o lado com "1 pinta" fica 
na primeira face definida, e o lado com "3  pintas" fica 
depois....

Para a primeira face do cubo / prisma:

```javascript
prisma.faceVertexUvs[0].push([u000v033.clone(),u025v033.clone(),u025v066.clone()]);
prisma.faceVertexUvs[0].push([u000v033.clone(),u025v066.clone(),u000v066.clone()]);
```

###passo 5: 
####Carregar a textura.

```javascript
url 	=  "textures/dado.png";
maptex  =  THREE.ImageUtils.loadTexture(url);
```

nota: o script deve correr a partir de um servidor (por exemplo local : localhost) sob pena de não funcionar devido a política de segurança dos browsers que impedem carregamento de ficheiros locais.

###passo 6: 
####Definir um material.

usar uma geometria numa "mesh" com um material básico e a indicação
do mapa de textura.
```javascript
var material = new THREE.MeshBasicMaterial({color : 0xFFFF0, map : maptex});
```

###passo 6: 
####criar a mesh e adicionar à cena

neste caso usar um objecto3D para incluir tudo

```javascript
mesh = new THREE.Mesh(prisma,material)
scene.add(mesh)
```

###passo 7:
####Apoio à Visualização.

Usar um "helper" para ter uma visualização das faces e um "helper" para os eixos 

###Final:
####Render

Fazer o render da cena de forma apropriada.



