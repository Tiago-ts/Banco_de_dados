# Banco_de_dados
Restaurante
create database restaurante;

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

CREATE SCHEMA IF NOT EXISTS `restaurante` DEFAULT CHARACTER SET utf8 ;

CREATE TABLE IF NOT EXISTS `restaurante`.`endereco` (
  `idendereco` INT(11) NOT NULL,
  `cidade` VARCHAR(45) NOT NULL,
  `logradouro` VARCHAR(45) NOT NULL,
  `UF` VARCHAR(45) NOT NULL,
  `cep` INT(11) NOT NULL,
  PRIMARY KEY (`idendereco`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

CREATE TABLE IF NOT EXISTS `restaurante`.`cliente` (
  `idcliente` INT(11) NOT NULL,
  `nome` VARCHAR(45) NULL DEFAULT NULL,
  `cpf` INT(11) NULL DEFAULT NULL,
  PRIMARY KEY (`idcliente`),
  UNIQUE INDEX `cpf_UNIQUE` (`cpf` ASC) )
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

CREATE TABLE IF NOT EXISTS `restaurante`.`usuario` (
  `idusuario` INT(11) NOT NULL,
  `login` VARCHAR(45) NOT NULL,
  `senha` VARCHAR(45) NOT NULL,
  `cliente_idcliente` INT(11) NOT NULL,
  `endereco_idendereco` INT(11) NOT NULL,
  PRIMARY KEY (`idusuario`),
  INDEX `fk_usuario_cliente1_idx` (`cliente_idcliente` ASC) ,
  INDEX `fk_usuario_endereco1_idx` (`endereco_idendereco` ASC) ,
  CONSTRAINT `fk_usuario_cliente1`
    FOREIGN KEY (`cliente_idcliente`)
    REFERENCES `restaurante`.`cliente` (`idcliente`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_usuario_endereco1`
    FOREIGN KEY (`endereco_idendereco`)
    REFERENCES `restaurante`.`endereco` (`idendereco`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

CREATE TABLE IF NOT EXISTS `restaurante`.`funcionario` (
  `idfuncionario` INT(11) NOT NULL,
  `nome` VARCHAR(45) NOT NULL,
  `data-admissao` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`idfuncionario`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

CREATE TABLE IF NOT EXISTS `restaurante`.`produto` (
  `idproduto` INT(11) NOT NULL AUTO_INCREMENT,
  `nome` VARCHAR(45) NOT NULL,
  `descricao` VARCHAR(45) NOT NULL,
  `valor` INT(11) NOT NULL,
  PRIMARY KEY (`idproduto`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

CREATE TABLE IF NOT EXISTS `restaurante`.`pagamento` (
  `idpagamento` INT(11) NOT NULL,
  `data-pagamento` DATE NOT NULL,
  `tipo` VARCHAR(45) NOT NULL,
  `usuario_idusuario` INT(11) NOT NULL,
  PRIMARY KEY (`idpagamento`),
  INDEX `fk_pagamento_usuario1_idx` (`usuario_idusuario` ASC) ,
  CONSTRAINT `fk_pagamento_usuario1`
    FOREIGN KEY (`usuario_idusuario`)
    REFERENCES `restaurante`.`usuario` (`idusuario`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

CREATE TABLE IF NOT EXISTS `restaurante`.`reserva` (
  `idreserva` INT(11) NOT NULL,
  `hora-data` DATETIME NULL DEFAULT NULL,
  `usuario_idusuario` INT(11) NOT NULL,
  `mesa_idmesa` INT(11) NOT NULL,
  PRIMARY KEY (`idreserva`),
  INDEX `fk_reserva_usuario1_idx` (`usuario_idusuario` ASC) ,
  INDEX `fk_reserva_mesa1_idx` (`mesa_idmesa` ASC),
  CONSTRAINT `fk_reserva_usuario1`
    FOREIGN KEY (`usuario_idusuario`)
    REFERENCES `restaurante`.`usuario` (`idusuario`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_reserva_mesa1`
    FOREIGN KEY (`mesa_idmesa`)
    REFERENCES `restaurante`.`mesa` (`idmesa`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

CREATE TABLE IF NOT EXISTS `restaurante`.`mesa` (
  `idmesa` INT(11) NOT NULL,
  `quantidade-lugares` INT(11) NULL DEFAULT NULL,
  PRIMARY KEY (`idmesa`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

CREATE TABLE IF NOT EXISTS `restaurante`.`pedido` (
  `usuario_idusuario` INT(11) NOT NULL,
  `produto_idproduto` INT(11) NOT NULL,
  `idpedido` INT(11) NOT NULL,
  `data` DATE NOT NULL,
  `funcionario_idfuncionario` INT(11) NOT NULL,
  PRIMARY KEY (`usuario_idusuario`, `produto_idproduto`, `idpedido`),
  INDEX `fk_usuario_has_produto_produto1_idx` (`produto_idproduto` ASC) ,
  INDEX `fk_usuario_has_produto_usuario_idx` (`usuario_idusuario` ASC) ,
  INDEX `fk_pedido_funcionario1_idx` (`funcionario_idfuncionario` ASC) ,
  CONSTRAINT `fk_usuario_has_produto_usuario`
    FOREIGN KEY (`usuario_idusuario`)
    REFERENCES `restaurante`.`usuario` (`idusuario`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_usuario_has_produto_produto1`
    FOREIGN KEY (`produto_idproduto`)
    REFERENCES `restaurante`.`produto` (`idproduto`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_pedido_funcionario1`
    FOREIGN KEY (`funcionario_idfuncionario`)
    REFERENCES `restaurante`.`funcionario` (`idfuncionario`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;

INSERT INTO `restaurante`.`cliente` (`idCliente`, `Nome`, `Cpf`) VALUES ('1', 'Eduardo bocao', '11111');
INSERT INTO `restaurante`.`cliente` (`idCliente`, `Nome`, `Cpf`) VALUES ('2', 'José play', '22222');
INSERT INTO `restaurante`.`cliente` (`idCliente`, `Nome`, `Cpf`) VALUES ('3', 'Latony', '33333');
INSERT INTO `restaurante`.`cliente` (`idCliente`, `Nome`, `Cpf`) VALUES ('4', 'Rodrigo', '44444');
INSERT INTO `restaurante`.`cliente` (`idCliente`, `Nome`, `Cpf`) VALUES ('5', 'Marcos machuca', '55555');
INSERT INTO `restaurante`.`cliente` (`idCliente`, `Nome`, `Cpf`) VALUES ('6', 'Will', '66666');
INSERT INTO `restaurante`.`cliente` (`idCliente`, `Nome`, `Cpf`) VALUES ('7', 'Amanda', '77777');
INSERT INTO `restaurante`.`cliente` (`idCliente`, `Nome`, `Cpf`) VALUES ('8', 'Renata', '88888');
INSERT INTO `restaurante`.`cliente` (`idCliente`, `Nome`, `Cpf`) VALUES ('9', 'Talysson', '99999');



INSERT INTO `restaurante`.`usuario` (`idUsuario`, `Login`, `Senha`, `Cliente_idCliente`, `endereco_idendereco`) VALUES ('1', '@Bocao', '1212', '1', '111');
INSERT INTO `restaurante`.`usuario` (`idusuario`, `login`, `senha`, `cliente_idcliente`, `endereco_idendereco`) VALUES ('2', '@jc', '1313', '2', '222');
INSERT INTO `restaurante`.`usuario` (`idusuario`, `login`, `senha`, `cliente_idcliente`, `endereco_idendereco`) VALUES ('3', '@latony', '1414', '3', '333');
INSERT INTO `restaurante`.`usuario` (`idusuario`, `login`, `senha`, `cliente_idcliente`, `endereco_idendereco`) VALUES ('4', '@rodriguete', '1515', '4', '444');
INSERT INTO `restaurante`.`usuario` (`idusuario`, `login`, `senha`, `cliente_idcliente`, `endereco_idendereco`) VALUES ('5', '@machuca', '1616', '5', '555');
INSERT INTO `restaurante`.`usuario` (`idusuario`, `login`, `senha`, `cliente_idcliente`, `endereco_idendereco`) VALUES ('6', '@willmaxixi', '1717', '6', '666');
INSERT INTO `restaurante`.`usuario` (`idusuario`, `login`, `senha`, `cliente_idcliente`, `endereco_idendereco`) VALUES ('7', '@mandinha', '1818', '7', '777');
INSERT INTO `restaurante`.`usuario` (`idusuario`, `login`, `senha`, `cliente_idcliente`, `endereco_idendereco`) VALUES ('8', '@reh', '1919', '8', '888');
INSERT INTO `restaurante`.`usuario` (`idusuario`, `login`, `senha`, `cliente_idcliente`, `endereco_idendereco`) VALUES ('9', '@talysson', '2020', '9', '999');


INSERT INTO `restaurante`.`endereco` (`idendereco`, `cidade`, `logradouro`, `UF`, `cep`) VALUES ('111', 'limoeiro', 'livramento', 'pe', '999888777');
INSERT INTO `restaurante`.`endereco` (`idendereco`, `cidade`, `logradouro`, `UF`, `cep`) VALUES ('222', 'vitória', 'iraque 2', 'pe', '114852369');
INSERT INTO `restaurante`.`endereco` (`idendereco`, `cidade`, `logradouro`, `UF`, `cep`) VALUES ('333', 'pombos', 'novo horizonte', 'pe', '741258963');
INSERT INTO `restaurante`.`endereco` (`idendereco`, `cidade`, `logradouro`, `UF`, `cep`) VALUES ('444', 'caruaru', 'cuscuz', 'pe', '369852147');
INSERT INTO `restaurante`.`endereco` (`idendereco`, `cidade`, `logradouro`, `UF`, `cep`) VALUES ('555', 'bonança', 'bela vista', 'pe', '852147963');
INSERT INTO `restaurante`.`endereco` (`idendereco`, `cidade`, `logradouro`, `UF`, `cep`) VALUES ('666', 'moreno', 'irá', 'pe', '147863259');
INSERT INTO `restaurante`.`endereco` (`idendereco`, `cidade`, `logradouro`, `UF`, `cep`) VALUES ('777', 'cha', 'iraque', 'pe', '456987123');
INSERT INTO `restaurante`.`endereco` (`idendereco`, `cidade`, `logradouro`, `UF`, `cep`) VALUES ('888', 'gravatá', 'matriz', 'pe', '951236487');
INSERT INTO `restaurante`.`endereco` (`idendereco`, `cidade`, `logradouro`, `UF`, `cep`) VALUES ('999', 'passira', 'alto', 'pe', '742356981');

INSERT INTO `restaurante`.`produto` (`idproduto`, `nome`, `descricao`, `valor`) VALUES ('1', 'lasanha', 'prato', '20');
INSERT INTO `restaurante`.`produto` (`idproduto`, `nome`, `descricao`, `valor`) VALUES ('2', 'pizza', 'prato', '15');
INSERT INTO `restaurante`.`produto` (`idproduto`, `nome`, `descricao`, `valor`) VALUES ('3', 'vinho', 'bebida', '10');
INSERT INTO `restaurante`.`produto` (`idproduto`, `nome`, `descricao`, `valor`) VALUES ('4', 'cerveja', 'bebida', '5');
INSERT INTO `restaurante`.`produto` (`idproduto`, `nome`, `descricao`, `valor`) VALUES ('5', 'churrasco', 'prato', '6');

INSERT INTO `restaurante`.`funcionario` (`idfuncionario`, `nome`, `data-admissao`) VALUES ('1', 'Maria', '2019-09-09');
INSERT INTO `restaurante`.`funcionario` (`idfuncionario`, `nome`, `data-admissao`) VALUES ('2', 'José', '2018-08-08');
INSERT INTO `restaurante`.`funcionario` (`idfuncionario`, `nome`, `data-admissao`) VALUES ('3', 'Joao', '2017-07-07');

INSERT INTO `restaurante`.`pedido` (`usuario_idusuario`, `produto_idproduto`, `idpedido`, `data`, `funcionario_idfuncionario`) VALUES ('1', '2', '1', '2019-09-09', '1');

select * from funcionario;

select * from endereco;

select * from produto;

select * from usuario;

select * from pedido;

select * from usuario;

select * from usuario u 
join pedido p 
on u.idusuario = p.idpedido;

select * from usuario u 
join pedido p 
on u.idusuario = p.idpedido
join produto pro 
on produto_idproduto = pro.idproduto;
