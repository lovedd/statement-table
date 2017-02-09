# statement-table
一个基于vue的含检索排序分页的表格和可以按条件过滤数据自动更新视图的图表。
## statement
#### 原料: `jquery.js` `Echart.js` `layui.js.` 
#### 快速上手(本例jsp)
* 1.)给省市区设置参数,本例是jsp :c foreach，下级js根据选择区域的code更新，方法不限。
```html
        <div class="layui-form-item">
        <label class="layui-form-label">区域：</label>
        <div class="layui-input-inline">
            <select name="province" lay-filter="province" id="province">
                <option value="">请选择省</option>
                <c:forEach var="pro" items="${areaList}" >
                    <option  value="${pro.code }" selected="">${pro.name}</option>
                </c:forEach>                
            </select>
        </div>
```
* 2.) 在`form.on`两个监听中写好省->市,市->区的ajax。
```js
        form.on('select(province)', function(data){
        $("#city").html("<option value=''>请选择市</option>");
        $("#area").html("<option value=''>请选择县/区</option>");
        $.ajax({
         type:"post",
          url:"<%=basePath%>statement/threelev.do",
          data:{"pro_id":data.value},
         success:function(data){
            //添加城市数据
            var html="<option value=''>请选择市</option>";
            for(var i=0;i<data.length;i++){
              html+="<option value='"+data[i].code+"'>"+data[i].name+"</option>";
            }
            $("#city").html(html);
            //重新渲染select
            form.render('select');
          },
          error: function(XMLHttpRequest, textStatus, errorThrown) {
               alert('意外错误')
          },
        }); 
        changeview();         
        });  
```
* 3.)`items_b1` `items_b2` `items_z1` `items_z2`是本例中的两个饼图和柱图的数据，通过工厂函数按需改成自己要的数据（如果修改记得在图表生成配置中也要同步修改）
* 4.)changeview()里获取新数据的ajax方法，本例提交后台4个参数`time` `starttime` `endtime` `areaCode`返回的数据按需修改，同理在`setoption`里按自己的数据需求修改。
* 5.)详细的我在demo里都写好注释了，赶紧试试吧
#### tips:
* 导出excel和pdf是后台代码，可自行添加。canvas中点击弹出的列表按钮通过echarts里的toolbox自定义，layer.open同过弹窗查看表格数据。
#### 效果预览:
![](statement.gif)
