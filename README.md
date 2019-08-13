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

 В сторе используем что-то вроде
 ```
 search: flow(function*(payload: SearchPayload) {
    const response = yield api.search(payload);
    const data: TData[] = response.data;

    return data; //ситуативно
  }),
 ```
 
В файле api
```
export function search(
  payload: TPayload,
  config?: AxiosRequestConfig
): AxiosPromise<any> {
  return axiosInstance.axios.get(
    "/...?" + queryString.stringify(payload),
    config
  );
}
```


# Async await
Вроде бы все правильно написано, но ничего не работает? Стоит проверить, стоит ли await в нужном месте. А именно, в том, где идет получение promise.

# Request
Если же сервер ничего не отдает или отдает не то, что нужно - куда именно отправляется запрос? Из цепочки 
axiosInstance - api.requests - store - component может выпасть MOC сервер, например axios-mock-adapter. Если он или подобное используется на проекте - проблема может быть именно там.

# React 
Функции упорно не видят свойства, методы, etc компонента? Возможно, необходимо больше спать и точно стоит проверить, аннимные ли они.
