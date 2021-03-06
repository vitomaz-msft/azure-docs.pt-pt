---
title: Ligar à Base de Dados do Azure para MySQL do PHP
description: Este guia de início rápido proporciona vários exemplos de código PHP que pode utilizar para se ligar e consultar dados da Base de Dados do Azure para MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.topic: quickstart
ms.date: 02/28/2018
ms.openlocfilehash: 7fa9272a8609d933a3f12abb0f33e78c4bdc1b12
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49984813"
---
# <a name="azure-database-for-mysql-use-php-to-connect-and-query-data"></a>Base de Dados do Azure para MySQL: utilizar o PHP para se ligar e consultar dados
Este guia de início rápido explica como se pode ligar a uma Base de Dados do Azure para MySQL através de uma aplicação [PHP](https://secure.php.net/manual/intro-whatis.php). Explica como utilizar as instruções SQL para consultar, inserir, atualizar e eliminar dados da base de dados. Este tópico pressupõe que está familiarizado com a programação com PHP e que nunca trabalhou com a Base de Dados do Azure para MySQL.

## <a name="prerequisites"></a>Pré-requisitos
Este guia de início rápido utiliza os recursos criados em qualquer um desTes guias como ponto de partida:
- [Criar uma Base de Dados do Azure para o servidor MySQL com o portal do Azure](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Criar uma Base de Dados do Azure para o servidor MySQL com a CLI do Azure](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-php"></a>Instalar o PHP
Instale o PHP no seu próprio servidor ou crie uma [aplicação Web](../app-service/app-service-web-overview.md) do Azure que inclua o PHP.

### <a name="macos"></a>MacOS
- Transfira a [versão 7.1.4 do PHP](https://secure.php.net/downloads.php).
- Instale o PHP e consulte o [manual do PHP](https://secure.php.net/manual/install.macosx.php) para mais configurações.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- Transfira a [versão 7.1.4 do PHP (x64) não segura para threads](https://secure.php.net/downloads.php).
- Instale o PHP e consulte o [manual do PHP](https://secure.php.net/manual/install.unix.php) para mais configurações.

### <a name="windows"></a>Windows
- Transfira a [versão 7.1.4 do PHP (x64) não segura para threads](https://windows.php.net/download#php-7.1).
- Instale o PHP e consulte o [manual do PHP](https://secure.php.net/manual/install.windows.php) para mais configurações.

## <a name="get-connection-information"></a>Obter informações da ligação
Obtenha as informações de ligação necessárias para se ligar à Base de Dados do Azure para MySQL. Necessita do nome do servidor e das credenciais de início de sessão totalmente qualificados.

1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).
2. No menu esquerdo do portal do Azure, clique em **Todos os recursos** e, em seguida, procure o servidor que acabou de criar, (por exemplo, **mydemoserver**).
3. Clique no nome do servidor.
4. No painel **Descrição geral** do servidor, tome nota do **Nome do servidor** e do **Nome de início de sessão de administrador do servidor**. Caso se esqueça da sua palavra-passe, também pode repor a palavra-passe neste painel.
 ![Nome do servidor da Base de Dados do Azure para o MySQL](./media/connect-php/1_server-overview-name-login.png)

## <a name="connect-and-create-a-table"></a>Ligar-se e criar uma tabela
Utilize o seguinte código para se ligar e crie uma tabela com a instrução SQL **CREATE TABLE**. 

O código utiliza a classe da **extensão MySQL melhorada** (mysqli) incluída em PHP. O código chama os métodos [mysqli_init](https://secure.php.net/manual/mysqli.init.php) e [mysqli_real_connect](https://secure.php.net/manual/mysqli.real-connect.php) para se ligar ao MySQL. Em seguida, chama o método [mysqli_query](https://secure.php.net/manual/mysqli.query.php) para executar a consulta. Em seguida, chama o método [mysqli_close](https://secure.php.net/manual/mysqli.close.php) para fechar a ligação.

Substitua os parâmetros host, username, password e db_name pelos seus próprios valores. 

```php
<?php
$host = 'mydemoserver.mysql.database.azure.com';
$username = 'myadmin@mydemoserver';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

// Run the create table query
if (mysqli_query($conn, '
CREATE TABLE Products (
`Id` INT NOT NULL AUTO_INCREMENT ,
`ProductName` VARCHAR(200) NOT NULL ,
`Color` VARCHAR(50) NOT NULL ,
`Price` DOUBLE NOT NULL ,
PRIMARY KEY (`Id`)
);
')) {
printf("Table created\n");
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="insert-data"></a>Inserir dados
Utilize o código seguinte para se ligar e inserir dados com uma instrução SQL **INSERT**.

O código utiliza a classe da **extensão MySQL melhorada** (mysqli) incluída em PHP. O código utiliza o método [mysqli_prepare](https://secure.php.net/manual/mysqli.prepare.php) para criar uma instrução de introdução preparada e, em seguida, une os parâmetros de cada valor introduzido na coluna através do método [mysqli_stmt_bind_param](https://secure.php.net/manual/mysqli-stmt.bind-param.php). O código executa a instrução através do método [mysqli_stmt_execute](https://secure.php.net/manual/mysqli-stmt.execute.php) e, depois, fecha a instrução através do método [mysqli_stmt_close](https://secure.php.net/manual/mysqli-stmt.close.php).

Substitua os parâmetros host, username, password e db_name pelos seus próprios valores. 

```php
<?php
$host = 'mydemoserver.mysql.database.azure.com';
$username = 'myadmin@mydemoserver';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Create an Insert prepared statement and run it
$product_name = 'BrandNewProduct';
$product_color = 'Blue';
$product_price = 15.5;
if ($stmt = mysqli_prepare($conn, "INSERT INTO Products (ProductName, Color, Price) VALUES (?, ?, ?)")) {
mysqli_stmt_bind_param($stmt, 'ssd', $product_name, $product_color, $product_price);
mysqli_stmt_execute($stmt);
printf("Insert: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

// Close the connection
mysqli_close($conn);
?>
```

## <a name="read-data"></a>Ler dados
Utilize o código seguinte para se ligar e ler dados com uma instrução SQL **SELECT**.  O código utiliza a classe da **extensão MySQL melhorada** (mysqli) incluída em PHP. O código utiliza o método [mysqli_query](https://secure.php.net/manual/mysqli.query.php) para executar a consulta sql e o método [mysqli_fetch_assoc](https://secure.php.net/manual/mysqli-result.fetch-assoc.php) para obter os registos daí resultantes.

Substitua os parâmetros host, username, password e db_name pelos seus próprios valores. 

```php
<?php
$host = 'mydemoserver.mysql.database.azure.com';
$username = 'myadmin@mydemoserver';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Select query
printf("Reading data from table: \n");
$res = mysqli_query($conn, 'SELECT * FROM Products');
while ($row = mysqli_fetch_assoc($res)) {
var_dump($row);
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="update-data"></a>Atualizar dados
Utilize o código seguinte para se ligar e atualizar os dados com uma instrução SQL **UPDATE**.

O código utiliza a classe da **extensão MySQL melhorada** (mysqli) incluída em PHP. O código utiliza o método [mysqli_prepare](https://secure.php.net/manual/mysqli.prepare.php) para criar uma instrução de atualização preparada e, em seguida, une os parâmetros de cada valor introduzido na coluna através do método [mysqli_stmt_bind_param](https://secure.php.net/manual/mysqli-stmt.bind-param.php). O código executa a instrução através do método [mysqli_stmt_execute](https://secure.php.net/manual/mysqli-stmt.execute.php) e, depois, fecha a instrução através do método [mysqli_stmt_close](https://secure.php.net/manual/mysqli-stmt.close.php).

Substitua os parâmetros host, username, password e db_name pelos seus próprios valores. 

```php
<?php
$host = 'mydemoserver.mysql.database.azure.com';
$username = 'myadmin@mydemoserver';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Update statement
$product_name = 'BrandNewProduct';
$new_product_price = 15.1;
if ($stmt = mysqli_prepare($conn, "UPDATE Products SET Price = ? WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 'ds', $new_product_price, $product_name);
mysqli_stmt_execute($stmt);
printf("Update: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));

//Close the connection
mysqli_stmt_close($stmt);
}

mysqli_close($conn);
?>
```


## <a name="delete-data"></a>Eliminar dados
Utilize o código seguinte para se ligar e ler os dados com a instrução SQL **DELETE**. 

O código utiliza a classe da **extensão MySQL melhorada** (mysqli) incluída em PHP. O código utiliza o método [mysqli_prepare](https://secure.php.net/manual/mysqli.prepare.php) para criar uma instrução de eliminação preparada e, em seguida, une os parâmetros da cláusula «onde» na instrução através do método [mysqli_stmt_bind_param](https://secure.php.net/manual/mysqli-stmt.bind-param.php). O código executa a instrução através do método [mysqli_stmt_execute](https://secure.php.net/manual/mysqli-stmt.execute.php) e, depois, fecha a instrução através do método [mysqli_stmt_close](https://secure.php.net/manual/mysqli-stmt.close.php).

Substitua os parâmetros host, username, password e db_name pelos seus próprios valores. 

```php
<?php
$host = 'mydemoserver.mysql.database.azure.com';
$username = 'myadmin@mydemoserver';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Delete statement
$product_name = 'BrandNewProduct';
if ($stmt = mysqli_prepare($conn, "DELETE FROM Products WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 's', $product_name);
mysqli_stmt_execute($stmt);
printf("Delete: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"]
> [Ligar à Base de Dados do Azure para MySQL através de SSL](howto-configure-ssl.md)
