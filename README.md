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

# Fetch and sort data with pagination with API for search
Достаточно простая логика для реализации поиска, сортировки с установкой типа и порядка сортировки и пагинацией для lazyload.
С чем стартуем (с учетом использования mobx):
```
searchString: string = "";

//пагинация
currentPage: number = INITIAL_PAGE_NUMBER; //номер страницы для загрузки
hasMorePage: boolean = true; //есть ли еще страницы

//сортировка
activeSortingType: SortingTypes = SortingTypes.TYPE;
activeSortingOrder: {increase: 0, decrease: 1} = SortingOrdersTypes.INCREASE;

//понадобится reset для пагинации
reset = () => {
  currentPage: number = INITIAL_PAGE_NUMBER;
  hasMorePage: boolean = true;
}

onChangeSorting = (newSortingType: SortingTypes) => {
  activeSortingType === newSortingType ?
    activeSortingOrder = +!activeSortingOrder //переключаем порядок сортировки
  :
    activeSortingType = newSortingType   
}

// to be continued...

```


# Async await
Вроде бы все правильно написано, но ничего не работает? Стоит проверить, стоит ли await в нужном месте. А именно, в том, где идет получение promise.

# Request
Если же сервер ничего не отдает или отдает не то, что нужно - куда именно отправляется запрос? Из цепочки 
axiosInstance - api.requests - store - component может выпасть MOC сервер, например axios-mock-adapter. Если он или подобное используется на проекте - проблема может быть именно там.

# React 
Функции упорно не видят свойства, методы, etc компонента? Возможно, необходимо больше спать и точно стоит проверить, не анонимные ли они.

# Reduce / Map / etc transform data
Бывают задачи вида "свести определенные элементы в массиве к одному элементу и подсчитать количество".
Один из вариантов, при сведении элементов из основного массиве вида `[1, 2, 3, 4, 5]` по подмассиву `[1, 3, 8]`, привести основной массив к виду `[[1, 3], 2, 4, 5]`, соответственно подмассив будет считаться как 1 элемент.  
Используется reduce по подмассиву, если элементы из подмассива есть в основном массиве - удаляем их из него, заносим в подмассив и возвращаем `[...mainArray, subArray]`.

В зависимости от контекста задачи и свойств элементов, если у элементов для сведения есть общее свойство с одинаовым значением, можно использовать хэш-таблицу, например, с помощью метода Set, не дающего создать дубликаты и, затем, подсчитать количество элементов в ней.
