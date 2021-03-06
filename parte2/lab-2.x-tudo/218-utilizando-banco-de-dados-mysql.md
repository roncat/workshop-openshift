### Persistindo dados em um banco de dados

#### Deploy de um MYSQL persistente através de um template

* Selecione no menu super "Add to project"
* Selecione a sub categoria "Data Stores" &gt; "MySQL \(Persistent\) 

![](/assets/Selection_165.png)

* Preencha os campos do template com os seguintes valores:
  * `Database Service Name` com     `mysql`
  * `MySQL Connection Username` com `redhat`
  * `MySQL Connection Password` com `redhat@123`
  * `MySQL root user Password` com  `redhat@123`
  * `MySQL Database Name com` com   `workshop`
  * `Volume Capacity com` com       `1Gi`

![](/assets/Selection_166.png)

O resultado deve ser similar a este

![](https://storage.googleapis.com/workshop-openshift/mysql-persistent.png)

> Observe através do menu lateral `Storage` que o volume para persistência dos registros no MySQL foi provisionado dinâmicamente

![](https://storage.googleapis.com/workshop-openshift/mysql-storage.png)

#### Altere a aplicação para apontar para o banco de dados persistente

Vamos mostrar uma lista de cidades cadastradas no banco de dados com essa aplicação.  
Para isso o primeiro passo será conectar a nossa aplicação já existente neste banco de dados, para isso  
precisamos adicionar o trecho de código abaixo ao arquivo `index.php` já existente.

```php
<?php
echo "<h1>Openshift Workshop v2.0</h1> ";
echo $_SERVER['SERVER_ADDR'];
echo "<br><hr>";
echo "<h2>Cidades cadastradas no Banco de Dados:</h2>";
$conn = new mysqli("mysql", "redhat", "redhat@123", "workshop");
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}
$result = $conn->query("SELECT nome FROM cidade");
if ($result->num_rows > 0) {
    while($row = $result->fetch_assoc()) {
        echo "<h3>" . $row["nome"] . "</h3>";
    }
} else {
    echo "0 results";
}
$conn->close();
?>
```

Como a tabela que está sendo consultada ainda não existe você estará vendo que existem zero cidades cadastradas neste momento.

![](https://storage.googleapis.com/workshop-openshift/app-zero-result.png)

#### Popule o banco de dados a partir da sua máquina local

Para criar a nossa tabela que a nossa aplicação está consumindo os dados, vamos utilizar o recurso de **port-forward**. Este  
recurso permite que uma porta TCP remota possa ser acessada como se estivesse disponível localmente  
através do uso de túnels.

```
oc login https://console.paas.rhbrlab.com -u <user>
oc get pods 
oc port-forward <pod id> 3306:3306
```

O comando `oc port-forward` mantém o terminal preso enquanto estiver executando. Para continuar com os próximos passos, será necessário abrir um segundo terminal no nosso servidor.

![](https://storage.googleapis.com/workshop-openshift/port-forward.png)

Neste momento temos o MYSQL disponível "localmente", podemos conectar a ele com ferramentas gráficas ou como será demonstrado aqui com o mysql client. Lembre-se de abrir um outro terminal ou colocar o processo do port-forward em background. Importante é que ele deve estar rodando antes de executar o comando abaixo:

```bash
mysql -u redhat -h 127.0.0.1 -p

Enter password: <insira aqui o password informado acima, redhat@123>

USE workshop;
CREATE TABLE cidade (id INT NOT NULL, nome VARCHAR(50) NOT NULL, PRIMARY KEY (id));
INSERT INTO cidade (id,nome) VALUES(1,"Rio de Janeiro");
INSERT INTO cidade (id,nome) VALUES(2,"Brasilia");
INSERT INTO cidade (id,nome) VALUES(3,"Recife");
SELECT * FROM cidade;
```

Ao acessar novamente a interface de nossas aplicação a mesma deverá estar mostrando a lista de cidades incluídas neste passo.

![](https://storage.googleapis.com/workshop-openshift/app-with-results.png)

Assim que todos os exercícios tiverem terminados, podemos parar o port-forward. Para isso, basta acessar o terminal em questão e executar um Ctrl + C.

![](/assets/Selection_164.png)

##### Mais informações

[https://docs.openshift.com/container-platform/3.6/dev\_guide/port\_forwarding.html](https://docs.openshift.com/container-platform/3.6/dev_guide/port_forwarding.html)

