# 📱 Quiz z Retrofitem – Lekcja 1

Aplikacja na Androida (Java), która **pobiera pytania z internetu** za pomocą biblioteki **Retrofit** i wyświetla je jako quiz ABCD. Na końcu pokazuje wynik i które odpowiedzi były dobre, a które złe.


## ✨ Co robi aplikacja?

- pobiera listę pytań z internetu (format JSON),
- wyświetla jedno pytanie i 4 odpowiedzi (A, B, C, D),
- po kliknięciu **„Potwierdź”** sprawdza odpowiedź i przechodzi dalej,
- jeśli nic nie zaznaczysz, pokazuje komunikat „Brak odpowiedzi”,
- na końcu wyświetla wynik, np. `Twój wynik to: 3/5`, oraz listę pytań z oceną **Dobrze / Źle**.


## 🔍 Działanie apki

### 1. Model – `Pytanie.java`
Klasa opisuje jedno pytanie. Nazwy pól muszą być takie same jak w JSON-ie, bo Gson dopasowuje je po nazwach.

```java
public class Pytanie {
    private String tresc;
    private String odpA, odpB, odpC, odpD;
    private int poprawna;   // numer poprawnej odpowiedzi (1-4)
    // konstruktor, gettery i settery
}
```


### 2. Interfejs API – JsonPlaceHolderApi.java
Mówimy Retrofitowi, jakie zapytanie ma wysłać. Tutaj jest jedno: `GET` pod adres `pytania`.

```java
public interface JsonPlaceHolderApi {
    @GET("pytania")
    Call<List<Pytanie>> getPytania();
}
```

### 3. Tworzenie Retrofita i pobieranie danych – `MainActivity.java`

```java
Retrofit retrofit = new Retrofit.Builder()
        .baseUrl("https://my-json-server.typicode.com/f4Mythical/json/")
        .addConverterFactory(GsonConverterFactory.create())
        .build();

JsonPlaceHolderApi api = retrofit.create(JsonPlaceHolderApi.class);
Call<List<Pytanie>> call = api.getPytania();

call.enqueue(new Callback<List<Pytanie>>() {
    @Override
    public void onResponse(Call<List<Pytanie>> call, Response<List<Pytanie>> response) {
        // dane przyszły – wyświetlamy pytanie
    }

    @Override
    public void onFailure(Call<List<Pytanie>> call, Throwable t) {
        // błąd (np. brak internetu)
    }
});
```

- `baseUrl` – adres serwera (musi kończyć się `/`),
- `GsonConverterFactory` – zamienia JSON na listę obiektów `Pytanie`,
- `enqueue` – zapytanie leci **w tle** (na Androidzie nie wolno robić zapytań sieciowych na głównym wątku).

### 4. Sprawdzanie odpowiedzi
Zaznaczony `RadioButton` jest zamieniany na liczbę 1–4 i porównywany z polem `poprawna`. Jeśli się zgadza, rośnie licznik `poprawne`.

---

## 🌐 Zródło pytań

Dane pochodzą z darmowego serwera testowego [My JSON Server](https://my-json-server.typicode.com/przykladowekonto/json). Przykładowy format jednego pytania:

```json
{
  "tresc": "Ile to jest 2 + 2?",
  "odpA": "3",
  "odpB": "4",
  "odpC": "5",
  "odpD": "22",
  "poprawna": 2
}
```


## 📚 Co jest w projekcie

- jak dodać Retrofita i Gsona w `build.gradle.kts`,
- jak zrobić klasę modelu pod dane z JSON-a,
- jak opisać zapytanie `@GET` w interfejsie,
- czym jest `enqueue` i dlaczego zapytania robi się asynchronicznie,
- jak dodać uprawnienie `INTERNET`.


