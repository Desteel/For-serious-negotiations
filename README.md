# For-serious-negotiations

Не очень серьезные и местами очевидные заметки по написанию кода.

# Fetch data with API

Простой вариант для загрузки данных с API

```
@observable private isLoading: boolean = false;

if (!this.isLoading) {
  this.isLoading = true;

  try {
    await Store.fetchData({
      search: this.search
    });
  } catch (error) {
    // error 
  } finally {
    this.isLoading = false;
  }
}
```
