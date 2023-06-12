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

### Codigo 2 de pruebas

```c++
int db_exec(sqlite3 *db, const char *sql) {
  Serial.println(sql);
  long start = millis();
  int rc = sqlite3_exec(db, sql, callback, (void*)data, &zErrMsg);
  if (rc != SQLITE_OK) {
    Serial.printf("SQL error: %s\n", zErrMsg);
    sqlite3_free(zErrMsg);
  } else {
    Serial.printf("Operation done successfully\n");
  }
  Serial.print(F("Time taken:"));
  Serial.println(millis() - start);
  return rc;
}

void setup() {
  //codigo
  Serial.begin(115200);
  sqlite3 *db1;
  int rc;
  SPI.begin();
  sqlite3_initialize();
  ///codigo

  if (db_open("/sd/usuario.db", &db1))
  {
    return;
  }

  String s_Command = "SELECT nombre FROM usuarios;";
  const char *c_Command = s_Command.c_str();

  rc = db_exec(db1, c_Command);

  Serial.println(rc);

  sqlite3_close(db1);

}

void loop() {

}
```
