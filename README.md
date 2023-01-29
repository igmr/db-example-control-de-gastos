# Control de gastos

Base de datos de ejemplo.

## Modelo entidad relación

![Modelo entidad relación](/docs/model.png)

## Script SQL

```sql

-- ****************************************************************************
-- * DATABASE                                                                 *
-- ****************************************************************************
DROP DATABASE IF EXISTS dbCostControl;
CREATE DATABASE IF NOT EXISTS dbCostControl;
USE dbCostControl;

-- ****************************************************************************
-- * TABLE CLASSIFICATIONS                                                          *
-- ****************************************************************************
DROP TABLE IF EXISTS classifications;
CREATE TABLE IF NOT EXISTS classifications
(
    id          INT                     NOT    NULL    AUTO_INCREMENT    COMMENT    '',
    name        VARCHAR(45)             NOT    NULL                      COMMENT    '',
    description VARCHAR(512)                   NULL    DEFAULT    NULL   COMMENT    '',
    icon        VARCHAR(65)                    NULL    DEFAULT    NULL   COMMENT    '',
    created_at  TIMESTAMP               NOT    NULL    DEFAULT    NOW()  COMMENT    '',
    updated_at  TIMESTAMP                      NULL    DEFAULT    NULL   COMMENT    '',
    deleted_at  TIMESTAMP                      NULL    DEFAULT    NULL   COMMENT    '',
    CONSTRAINT  pk_classifications      PRIMARY KEY(id),
    CONSTRAINT  uk_id_classifications   UNIQUE(id),
    CONSTRAINT  uk_name_classifications UNIQUE(name)
) ENGINE=INNODB CHARSET=utf8mb4;

-- ****************************************************************************
-- * TABLE SUBCLASSIFICATIONS                                                       *
-- ****************************************************************************
DROP TABLE IF EXISTS subclassifications;
CREATE TABLE IF NOT EXISTS subclassifications
(
    id                   INT              NOT    NULL    AUTO_INCREMENT    COMMENT    '',
    classification_id    INT              NOT    NULL    DEFAULT    2      COMMENT    '',
    name                 VARCHAR(45)      NOT    NULL                      COMMENT    '',
    description          VARCHAR(512)            NULL    DEFAULT    NULL   COMMENT    '',
    icon                 VARCHAR(65)             NULL    DEFAULT    NULL   COMMENT    '',
    created_at           TIMESTAMP        NOT    NULL    DEFAULT    NOW()  COMMENT    '',
    updated_at           TIMESTAMP               NULL    DEFAULT    NULL   COMMENT    '',
    deleted_at           TIMESTAMP               NULL    DEFAULT    NULL   COMMENT    '',
    CONSTRAINT pk_subclassifications      PRIMARY KEY(id),
    CONSTRAINT fk_classifications         FOREIGN KEY(classification_id)
        REFERENCES classifications(id),
    CONSTRAINT uk_id_subclassifications   UNIQUE(id),
    CONSTRAINT uk_name_subclassifications UNIQUE(name)
) ENGINE=INNODB CHARSET=utf8mb4;

-- ****************************************************************************
-- * TABLE OPERATIONS                                                               *
-- ****************************************************************************
DROP TABLE IF EXISTS operations;
CREATE TABLE IF NOT EXISTS operations
(
    id                      INT             NOT    NULL    AUTO_INCREMENT        COMMENT    '',
    subclassification_id    INT             NOT    NULL    DEFAULT    0          COMMENT    '',
    type                    ENUM('income'
                                ,'outcome') NOT    NULL    DEFAULT    'outcome'  COMMENT    '',
    amount                  FLOAT           NOT    NULL    DEFAULT    0          COMMENT    '',
    description             VARCHAR(512)           NULL    DEFAULT    NULL       COMMENT    '',
    created_at              TIMESTAMP       NOT    NULL    DEFAULT    NOW()      COMMENT    '',
    updated_at              TIMESTAMP              NULL    DEFAULT    NULL       COMMENT    '',

    CONSTRAINT pk_operations                PRIMARY KEY(id),
    CONSTRAINT fk_subclassifications        FOREIGN KEY(subclassification_id)
        REFERENCES subclassifications(id),
    CONSTRAINT uk_id_operations             UNIQUE(id)
) ENGINE=INNODB CHARSET=utf8mb4;

-- ****************************************************************************
-- * INSERTS TABLE CLASSIFICATIONS
-- ****************************************************************************
INSERT INTO classifications(name, description, icon, created_at)
VALUES
    ('Ingreso'                   ,    NULL,    NULL,    NOW()),
    ('(Ninguno)'                 ,    NULL,    NULL,    NOW()),
    ('Vivienda'                  ,    NULL,    NULL,    NOW()),
    ('Comida'                    ,    NULL,    NULL,    NOW()),
    ('Impuestos y donaciones'    ,    NULL,    NULL,    NOW()),
    ('Transporte'                ,    NULL,    NULL,    NOW()),
    ('Seguros'                   ,    NULL,    NULL,    NOW()),
    ('Ahorros e Inversiones'     ,    NULL,    NULL,    NOW()),
    ('Servicios'                 ,    NULL,    NULL,    NOW()),
    ('Salud'                     ,    NULL,    NULL,    NOW()),
    ('Vestimenta'                ,    NULL,    NULL,    NOW()),
    ('Recreación'                ,    NULL,    NULL,    NOW()),
    ('Personal'                  ,    NULL,    NULL,    NOW()),
    ('Deudas'                    ,    NULL,    NULL,    NOW());

-- ****************************************************************************
-- * INSERTS TABLE SUBCLASSIFICATIONS
-- ****************************************************************************
INSERT INTO subclassifications(classification_id, name, description, icon, created_at)
VALUES
    (001,"Ingreso"                        ,    NULL,    NULL,    NOW()),

    (002,"(Ninguno)"                      ,    NULL,    NULL,    NOW()),

    (003,"Hipoteca Uno/Renta"             ,    NULL,    NULL,    NOW()),
    (003,"Hipoteca Dos"                   ,    NULL,    NULL,    NOW()),
    (003,"Impuestos de vivienda"          ,    NULL,    NULL,    NOW()),
    (003,"Reparaciones/Mantenimiento"     ,    NULL,    NULL,    NOW()),
    (003,"Gastos de administración"       ,    NULL,    NULL,    NOW()),

    (004,"Despensa"                       ,    NULL,    NULL,    NOW()),
    (004,"Restaurantes"                   ,    NULL,    NULL,    NOW()),

    (005,"Impuestos"                      ,    NULL,    NULL,    NOW()),
    (005,"Donaciones"                     ,    NULL,    NULL,    NOW()),

    (006,"Gasolina & Aceite"              ,    NULL,    NULL,    NOW()),
    (006,"Reparación & Llantas"           ,    NULL,    NULL,    NOW()),
    (006,"Licencia & Impuestos"           ,    NULL,    NULL,    NOW()),
    (006,"Reemplazo de Auto"              ,    NULL,    NULL,    NOW()),
    (006,"Transporte público"             ,    NULL,    NULL,    NOW()),

    (007,"Seguro de vida"                 ,    NULL,    NULL,    NOW()),
    (007,"Seguro de gastos médicos"       ,    NULL,    NULL,    NOW()),
    (007,"Seguro de vivienda"             ,    NULL,    NULL,    NOW()),
    (007,"Seguro de auto"                 ,    NULL,    NULL,    NOW()),
    (007,"Seguro de incapacidad"          ,    NULL,    NULL,    NOW()),
    (007,"Seguro contra robo"             ,    NULL,    NULL,    NOW()),
    (007,"Cuidados a largo plazo"         ,    NULL,    NULL,    NOW()),

    (008,"Fondo de emergencias"           ,    NULL,    NULL,    NOW()),
    (008,"Fondo para el retiro"           ,    NULL,    NULL,    NOW()),
    (008,"Fondo para estudios"            ,    NULL,    NULL,    NOW()),

    (009,"Electricidad"                   ,    NULL,    NULL,    NOW()),
    (009,"Gas"                            ,    NULL,    NULL,    NOW()),
    (009,"Agua"                           ,    NULL,    NULL,    NOW()),
    (009,"Servicios de limpieza"          ,    NULL,    NULL,    NOW()),
    (009,"Teléfono"                       ,    NULL,    NULL,    NOW()),
    (009,"Internet"                       ,    NULL,    NULL,    NOW()),
    (009,"Cable"                          ,    NULL,    NULL,    NOW()),

    (010,"Medicamentos"                   ,    NULL,    NULL,    NOW()),
    (010,"Médicos"                        ,    NULL,    NULL,    NOW()),
    (010,"Dentista"                       ,    NULL,    NULL,    NOW()),
    (010,"Optometrista"                   ,    NULL,    NULL,    NOW()),
    (010,"Vitaminas"                      ,    NULL,    NULL,    NOW()),
    (010,"Salud otros 1"                  ,    NULL,    NULL,    NOW()),
    (010,"Salud otros 2"                  ,    NULL,    NULL,    NOW()),

    (011,"Adultos"                        ,    NULL,    NULL,    NOW()),
    (011,"Niños"                          ,    NULL,    NULL,    NOW()),
    (011,"Limpieza"                       ,    NULL,    NULL,    NOW()),

    (012,"Entretenimiento"                ,    NULL,    NULL,    NOW()),
    (012,"Vacaciones"                     ,    NULL,    NULL,    NOW()),

    (013,"Guardería/Niñera"               ,    NULL,    NULL,    NOW()),
    (013,"Artículos de tocador"           ,    NULL,    NULL,    NOW()),
    (013,"Cosméticos/Cuidado del cabello" ,    NULL,    NULL,    NOW()),
    (013,"Educación/Colegiatura"          ,    NULL,    NULL,    NOW()),
    (013,"Libros/Útiles"                  ,    NULL,    NULL,    NOW()),
    (013,"Manutención"                    ,    NULL,    NULL,    NOW()),
    (013,"Pensión alimenticia"            ,    NULL,    NULL,    NOW()),
    (013,"Suscripciones"                  ,    NULL,    NULL,    NOW()),
    (013,"Gastos de organización"         ,    NULL,    NULL,    NOW()),
    (013,"Regalos"                        ,    NULL,    NULL,    NOW()),
    (013,"Reemplazar muebles"             ,    NULL,    NULL,    NOW()),
    (013,"Dinero de bolsillo (de el)"     ,    NULL,    NULL,    NOW()),
    (013,"Dinero de bolsillo (de ella)"   ,    NULL,    NULL,    NOW()),
    (013,"Artículos para bebé"            ,    NULL,    NULL,    NOW()),
    (013,"Artículos para mascota"         ,    NULL,    NULL,    NOW()),
    (013,"Música/Tecnología"              ,    NULL,    NULL,    NOW()),
    (013,"Varios"                         ,    NULL,    NULL,    NOW()),
    (013,"Contador"                       ,    NULL,    NULL,    NOW()),
    (013,"Personal otro 1"                ,    NULL,    NULL,    NOW()),
    (013,"Personal otro 2"                ,    NULL,    NULL,    NOW()),

    (014,"Pago de auto 1"                 ,    NULL,    NULL,    NOW()),
    (014,"Pago de auto 2"                 ,    NULL,    NULL,    NOW()),
    (014,"Tarjeta de crédito 1"           ,    NULL,    NULL,    NOW()),
    (014,"Tarjeta de crédito 2"           ,    NULL,    NULL,    NOW()),
    (014,"Tarjeta de crédito 3"           ,    NULL,    NULL,    NOW()),
    (014,"Tarjeta de crédito 4"           ,    NULL,    NULL,    NOW()),
    (014,"Tarjeta de crédito 5"           ,    NULL,    NULL,    NOW()),
    (014,"Préstamo estudiantil 1"         ,    NULL,    NULL,    NOW()),
    (014,"Préstamo estudiantil 2"         ,    NULL,    NULL,    NOW()),
    (014,"Préstamo estudiantil 3"         ,    NULL,    NULL,    NOW()),
    (014,"Préstamo estudiantil 4"         ,    NULL,    NULL,    NOW()),
    (014,"Deuda otro 1"                   ,    NULL,    NULL,    NOW()),
    (014,"Deuda otro 2"                   ,    NULL,    NULL,    NOW()),
    (014,"Deuda otro 3"                   ,    NULL,    NULL,    NOW()),
    (014,"Deuda otro 4"                   ,    NULL,    NULL,    NOW()),
    (014,"Deuda otro 5"                   ,    NULL,    NULL,    NOW());

-- ****************************************************************************
-- * INSERTS TABLE OPERATIONS
-- ****************************************************************************
INSERT INTO operations (subclassification_id, type, amount, description, created_at)
VALUES
(1, 'income' ,  100, 'Ingreso demo', now()),
(2, 'outcome', -100, 'Outcome demo', now());
-- ****************************************************************************

```
