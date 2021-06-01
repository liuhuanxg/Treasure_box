## DataTable的使用

官网链接：https://datatables.net/

一、引入

```html
<link href ="https://cdn.datatables.net/1.10.20/css/jquery.dataTables.min.css">

<script src="https://cdn.datatables.net/1.10.20/js/jquery.dataTables.min.js"></script>

```

二、使用

```javascript
<script>
      $(document).ready( function () {
          $('#table_0').DataTable({   /*使用id进行绑定*/
              "ordering": false,  /*关闭插件自带排序*/
              "pageLength": 30,      /*每页数目*/
              language: {
                  "sProcessing": "处理中...",
                  "sLengthMenu": "显示 _MENU_ 条记录",
                  "sZeroRecords": "没有匹配结果",
                  "sInfo": "显示第 _START_ 至 _END_ 条记录，共 _TOTAL_ 条提测记录",
                  "sInfoEmpty": "显示第 0 至 0 条记录，共 0 条",
                  "sInfoFiltered": "(由 _MAX_ 条记录过滤)",
                  "sInfoPostFix": "",
                  "sSearch": "搜索:",
                  "sUrl": "",
                  "sEmptyTable": "表中数据为空",
                  "sLoadingRecords": "载入中...",
                  "sInfoThousands": ",",
                  "oPaginate": {
                      "sFirst": "首页",
                      "sPrevious": "<",
                      "sNext": ">",
                      "sLast": "末页"
                   },
                  "oAria": {
                      "sSortAscending": ": 以升序排列此列",
                      "sSortDescending": ": 以降序排列此列"
                  }
              }
           });
       });
    </script>

```

