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
