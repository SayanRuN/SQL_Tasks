_Создание таблицы_
```
CREATE TABLE Pieces (
   Code INTEGER PRIMARY KEY NOT NULL,
   Name TEXT NOT NULL
 );
 
 CREATE TABLE Providers (
  Code TEXT PRIMARY KEY NOT NULL,
  Name TEXT NOT NULL
 );
 
 CREATE TABLE Provides (
   Piece INTEGER  
     CONSTRAINT fk_Pieces_Code REFERENCES Pieces(Code),
   Provider TEXT
     CONSTRAINT fk_Providers_Code REFERENCES Providers(Code),
   Price INTEGER NOT NULL,
   PRIMARY KEY(Piece, Provider)
 );
```
***

_Внесение значений_
```
 INSERT INTO Providers(Code, Name) VALUES('HAL','Clarke Enterprises');
 INSERT INTO Providers(Code, Name) VALUES('RBT','Susan Calvin Corp.');
 INSERT INTO Providers(Code, Name) VALUES('TNBC','Skellington Supplies');
 
 INSERT INTO Pieces(Code, Name) VALUES(1,'Sprocket');
 INSERT INTO Pieces(Code, Name) VALUES(2,'Screw');
 INSERT INTO Pieces(Code, Name) VALUES(3,'Nut');
 INSERT INTO Pieces(Code, Name) VALUES(4,'Bolt');
 
 INSERT INTO Provides(Piece, Provider, Price) VALUES(1,'HAL',10);
 INSERT INTO Provides(Piece, Provider, Price) VALUES(1,'RBT',15);
 INSERT INTO Provides(Piece, Provider, Price) VALUES(2,'HAL',20);
 INSERT INTO Provides(Piece, Provider, Price) VALUES(2,'RBT',15);
 INSERT INTO Provides(Piece, Provider, Price) VALUES(2,'TNBC',14);
 INSERT INTO Provides(Piece, Provider, Price) VALUES(3,'RBT',50);
 INSERT INTO Provides(Piece, Provider, Price) VALUES(3,'TNBC',45);
 INSERT INTO Provides(Piece, Provider, Price) VALUES(4,'HAL',5);
 INSERT INTO Provides(Piece, Provider, Price) VALUES(4,'RBT',7);
```
***

_Задачи_

1. Select the name of all the pieces.
2. Select all the providers' data.
3. Obtain the average price of each piece (show only the piece code and the average price).
4. Obtain the names of all providers who supply piece 1.
5. Select the name of pieces provided by provider with code "HAL".
6. For each piece, find the most expensive offering of that piece and include the piece name, provider name, and price (note that there could be two providers who supply the same piece at the most expensive price).
7. Add an entry to the database to indicate that "Skellington Supplies" (code "TNBC") will provide sprockets (code "1") for 7 cents each.
8. Increase all prices by one cent.
9. Update the database to reflect that "Susan Calvin Corp." (code "RBT") will not supply bolts (code 4).
10. Update the database to reflect that "Susan Calvin Corp." (code "RBT") will not supply any pieces (the provider should still remain in the database).
***

_Решение_
```
SELECT * FROM pieces;

SELECT * FROM providers;

SELECT piece, avg(price) FROM provides
GROUP BY piece;
SELECT pp.name FROM provides p, providers pp
WHERE pp.code=p.provider AND p.piece=1;
SELECT name FROM pieces

INNER JOIN provides
ON pieces.code = provides.piece
WHERE provider='HAL';

SELECT ps.name, pr.name, pd.price FROM pieces ps, providers pr, provides pd
WHERE ps.code=pd.piece AND pr.code=pd.provider AND pd.price=(SELECT max(price) FROM provides WHERE ps.code=provides.piece);

INSERT INTO provides (piece, provider, price) VALUES (1, 'TNBC', 7);

UPDATE provides SET price=price+1;

DELETE FROM provides
WHERE piece=4 AND provider='RBT';

DELETE FROM provides
WHERE provider='RBT';
```