<html>
<head>
    <title>2015726044이다인</title>
    <script type = "text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.7.2/Chart.js"></script>
    <style type="text/css">
        #calculator {
            width: 550px;
        }
       
        #result {
            width: 323px;
            height: 28px;
            background-color: black; 
            border:13px solid black;      
            font: 33px digital;
            color: white;
            font-variant-position: sub;
        }
        #result1,#result2,#result3 {
            width: 335px;
            height: 20px;
            background-color: black; 
            border:7px solid black;   
            font: 20px digital;
            color: dimgrey;
        }
        #record{
            width: 335px;
            height: 155px;
            background-color:black;
            border:7px solid black;   
            font: 15px digital;
            color:white;
        }
        #graph{
            width: 348px;
            height: 160px;
            background-color:black;
            border:0px solid black;   
            font: 15px digital;
            color:white;
        }
        .key,.key1,.key3,.key4,.num {
            display: inline-block;          
            width: 65px;
            height: 30px;
            border-radius:5px;
            border: 1px solid black;   
            background-color:dimgrey;
            color:white;
        }
        .key2{
            display: inline-block;          
            width: 27.5px;
            height: 28px;
            border-radius:5px;
            border: 0px solid black; 
            color :black;
            background-color: white;    
            font: bold 15px sans-serif;
        }
        .num {
            color :black;
            background-color: white;    
            font: bold 15px sans-serif;
        }
        .key1 {
            color :black;
            background-color:lightseagreen;    
            font: bold 15px sans-serif;
        }
        .key4{
            color: black;
            background-color:orange;    
            font: bold 15px sans-serif;
        }

        .chartWrapper{
            position: relative;
        }
        .chartWrapper > canvas{
            position: absolute;
            left: 0;
            top: 0;
            pointer-events: none;
        }
        .chartAreaWrapper{
            width: 348px;
            overflow-x: scroll;
        }
    </style>

    <script src='https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js' type='text/javascript'></script>  
    <script src='math.js' type='text/javascript'></script>
     
    <script type="text/javascript">
        
        var myData = {
            labels:["-50","-45","-40","-35","-30","-25","-20","-15","-10","-5","0","5","10","15","20","25","30","35","40","45","50","55","60","65","70","75","80"],
            datasets:[{
                label:'-',
                data:[-1000],
                borderColor: "red",
                fill: false,
                borderWidth: 1
            },{
                label:'-',
                data:[-1000],
                borderColor: "green",
                fill: false,
                borderWidth: 1
            }]
        };
        var myOption ={
            responsive: false,
                scales: {
                    yAxes: [{
                        ticks: {
                            max:200, min:-200, stepSize:20
                        }
                    }]
                }
        }
        
        
        var fnNum =0;
    //////////////////////////////////////////////////////////////////
        $(document).ready(function(){
            var parser = math.parser();
            var ctx = document.getElementById("graph").getContext('2d');
            var myChart = new Chart(ctx,{
                type:'line',
                data: myData,
                options: myOption,
                //scaleGridLineColor : "rgba(0,0,0,0.05)"
            });

            var realValue = '';
            var displayValue ='';
            var recordValue = '';
            $('#result').text(realValue);
            $('#result1').text(realValue);
            $('#result2').text(realValue);
            $('#record').hide();
            $('.chartWrapper').hide();
 
            $('.key,.num,.key1,.key3,.key2,.key4').each(function(index, key){       
                $(this).dblclick(function(e){   //더블클릭시 모두 지워짐
                    if($(this).text() == 'CL')
                    {
                        realValue = '';
                        displayValue = '';
                        $('#result').text(realValue);
                        $('#result1').text(realValue);
                        $('#result2').text(realValue);
                    }
                })
                $(this).click(function(e){
                    
                    if($(this).text() == '계산'){
                        $('#record').hide();
                        $('.chartWrapper').hide();
                        
                    }
                    else if($(this).text() == '기록'){
                        $('#record').show();
                        $('.chartWrapper').hide();
                    }
                    else if($(this).text() == '그래프'){
                        $('#record').hide();
                        $('.chartWrapper').show();
                    }
                    else if($(this).text() == 'EV')
                    {
                        try
                        {
                            var fn = realValue[0];
                            realValue = parser.eval(realValue).toString();
                            var tokens = realValue.split(' ');
                            if(tokens[0] == 'function')
                            {
                                realValue = tokens[0];
                                $('#result2').text("- "+displayValue);

                                var node = document.createElement("LI");                 
                                var textnode = document.createTextNode(displayValue);         
                                node.appendChild(textnode);                              
                                document.getElementById("record").appendChild(node); 

                                if(fnNum==2) fnNum=0;
                                myData.datasets[fnNum].label = displayValue;
                                var add_Data = myData.datasets[fnNum].data;
                                var add_label = myData.labels;
                                for(var i=0; i<add_label.length; i++){
                                    add_Data[i] = parser.eval(fn+"("+add_label[i]+")");
                                }
                                myData.datasets[fnNum].data = add_Data;
                                myChart.update();
                                fnNum++;
                                
                                

                            }
                            else{  
                                $('#result1').text("- "+displayValue+"="+realValue);

                                
                                var node = document.createElement("LI");                 
                                var textnode = document.createTextNode(displayValue+"="+realValue);         
                                node.appendChild(textnode);                              
                                document.getElementById("record").appendChild(node);    
                            }       
                            $('#result').text(realValue);             
                            realValue = '';
                            displayValue = realValue;
                        }
                        catch (e)
                        {
                            realValue = '';
                            displayValue = '';
                            if(realValue != 'function')
                            {
                                $('#result').text(e);
                            }
                        }               
                    }
                    else
                    {
                        if($(this).text() == 'CL')
                        {
                            realValue = '';
                            displayValue = '';
                            $('#result').text(displayValue);
                        }
                        else if($(this).val() == 'c')
                        {
                            realValue = realValue.substring(0,realValue.length-1);
                            displayValue = displayValue.substring(0,displayValue.length-1);
                            $('#result').text(displayValue);
                        }
                        else if($(this).attr('class')=='key3')
                        {
                            realValue += $(this).val();
                            displayValue = displayValue + $(this).text()+"(";
                            $('#result').text(displayValue);
                        }
                        else if($(this).attr('class')=='move')
                        {
                            $('#result').selectRange(3,3);
                        }
                        else
                        {                        
                            realValue += $(this).val();
                            displayValue += $(this).text();
                            $('#result').text(displayValue);
                        }
                    }
                })
            })
        })
    </script>
</head>
 
<body>
    <table  style="background-color: black; position: absolute; left:8px;top:8px; z-index: 1; ">
            <tr>
                    <td><button class="key4" value="계산">계산</button></td>
                    <td><button class="key4" value="기록">기록</button></td>
                    <td><button class="key4" value="그래프">그래프</button></td>
                    <td><button class="key1" value="c">◀</button></td>
                    <td><button class="key1" value="CL">CL</button></td>
                </tr>
    </table>
    <div class = "chartWrapper" height="100px" width="355px" style="background-color: red;position: absolute; left:8px;top:40px; z-index: 1">
        <div class="chartAreaWrapper">
                <canvas id="graph" height="485px" width="1000px"></canvas>
        </div>
        <canvas id="graphAxis" height="0px" width="0"></canvas>
    </div>

    <div id="record" style="position: absolute; left:8px;top:40px; z-index: 1; overflow-y:scroll;"></div>
    <div style="position: absolute; left:8px;top:40px;  ">
    <table style="background-color: black;">
            
        <tr>           
            <div id="result2"></div>
        </tr>
        <tr>
            <div id="result1"></div>
        </tr>
        <tr>
            <div id="result3"></div>
        </tr>
        <tr>
            <div id="result"></div>
        </tr>
        <tr>
            
            <tr>
                <td><button class="key3" value="f(">f</button></td>
                <td><button class="key3" value="g(">g</button></td>
                <td><button class="key" value="x">x</button></td>
                <td><button class="key" value="y">y</button></td>
                <td><button class="key" value="z">z</button></td>
            </tr>
            <tr>
                <td><button class="key" value="PI">π</button></td>
                <td><button class="key" value="E">e</button></td>
                <td><button class="key" value="A">A</button></td>
                <td><button class="key" value="B">B</button></td>
                <td><button class="key" value="C">C</button></td>
            </tr>
            <tr>
                <td><button class="key3" value="dot(">내적</button></td>
                <td><button class="key3" value="cross(">외적</button></td>
                <td><button class="key3" value="inv(">역행렬</button></td>
                <td><button class="key3" value="det(">det</button></td>
                <td><button class="key3" value="pow(">pow</button></td>
            </tr>
            <tr>
                <td><button class="key3" value="sin(">sin</button></td>
                <td><button class="key3" value="cos(">cos</button></td>
                <td><button class="key3" value="tan(">tan</button></td>
                 <td><button class="key3" value="asin(">asin</button></td>
                <td><button class="key3" value="acos(">acos</button></td>
            </tr>
            <tr>
                <td><button class="key3" value="sqrt(">²√</button></td>
                <td><button class="key3" value="cbrt(">³√</button></td>
                <td><button class="key3" value="exp(">e^</button></td>
                <td><button class="key3" value="log(">log</button></td>
                <td><button class="key3" value="log10(">log10</button></td>
            </tr>
            <tr>
                <td><button class="key1" value="+">+</button></td>
                <td><button class="key1" value="-">-</button></td>
                <td><button class="key1" value="*">*</button></td>
                <td><button class="key1" value="/">/</button></td>
                <td><button class="key1" value="%">%</button></td>
            </tr>
            <tr>
                <td><button class="key1" value="!">!</button></td>
                <td><button class="key1" value="=">=</button></td>
                <td><button class="num" value="7">7</button></td>
                <td><button class="num" value="8">8</button></td>
                <td><button class="num" value="9">9</button></td>
            </tr>
            <tr>
                <td><button class="key1" value="<"><</button></td>
                <td><button class="key1" value=">">></button></td>
                <td><button class="num" value="4">4</button></td>
                <td><button class="num" value="5">5</button></td>
                <td><button class="num" value="6">6</button></td>
            </tr>
            <tr>
                <td><button class="key1" value="[">[</button></td>
                <td><button class="key1" value="]">]</button></td>
                <td><button class="num" value="1">1</button></td>
                <td><button class="num" value="2">2</button></td>
                <td><button class="num" value="3">3</button></td>
            </tr>
            <tr>
                <td><button class="key1" value="(">(</button></td>
                <td><button class="key1" value=")">)</button></td>
                <td><button class="num" value="0">0</button></td>
                <td>
                    <table>
                        <td><button class="key2" value=",">,</button></td>
                        <td><button class="key2" value=".">.</button></td>
                    </table>
                </td>
                <td><button class="key1" value="EV" style="
                    color: black;
                    background-color:orange;    
                    font: bold 15px sans-serif">EV</button></td>
            </tr>
           
            
        </tr>
    </table>
    </div>
</body>
</html>
