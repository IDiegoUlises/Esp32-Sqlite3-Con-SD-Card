# Esp32-Sqlite3-Con-SD-Card

### Codigo de pruebas
```c++
void setup() {
  Serial.begin(115200);
  delay(5000);

  sqlite3 *db1;
  int rc;

  SPI.begin();

  sqlite3_initialize();

  if (db_open("/SD/user.db", &db1))
  {
    Serial.println("Abierto no se que hace");
  }


  rc = db_exec(db1, "SELECT nombre FROM usuarios");

  if (rc != SQLITE_OK)
  {
    sqlite3_close(db1);
    Serial.println("sqlite ok salida");
    return;
  }

}

void loop() {

}

```
