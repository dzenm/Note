# android实现计时与倒计时的五种方法

###方法一

* Timer与TimerTask（Java实现）

```
public class timerTask extends Activity{    
   
    private int recLen = 11;    
    private TextView txtView;    
    Timer timer = new Timer();    
   
    public void onCreate(Bundle savedInstanceState){    
        super.onCreate(savedInstanceState);    
            
        setContentView(R.layout.timertask);    
        txtView = (TextView)findViewById(R.id.txttime);    
            
        timer.schedule(task, 1000, 1000);       // timeTask    
    }       
   
    TimerTask task = new TimerTask() {    
        @Override    
        public void run() {    
   
            runOnUiThread(new Runnable() {      // UI thread    
                @Override    
                public void run() {    
                    recLen--;    
                    txtView.setText(""+recLen);    
                    if(recLen < 0){    
                        timer.cancel();    
                        txtView.setVisibility(View.GONE);    
                    }    
                }    
            });    
        }    
    };    
}
```

###方法二 
* TimerTask与Handler（不用Timer的改进型）

```
public class timerTask extends Activity{    
    private int recLen = 11;    
    private TextView txtView;    
    Timer timer = new Timer();    
   
    public void onCreate(Bundle savedInstanceState){    
        super.onCreate(savedInstanceState);    
   
        setContentView(R.layout.timertask);    
        txtView = (TextView)findViewById(R.id.txttime);    
   
        timer.schedule(task, 1000, 1000);       // timeTask    
    }       
   
    final Handler handler = new Handler(){    
        @Override    
        public void handleMessage(Message msg){    
            switch (msg.what) {    
            case 1:    
                txtView.setText(""+recLen);    
                if(recLen < 0){    
                    timer.cancel();    
                    txtView.setVisibility(View.GONE);    
                }    
            }    
        }    
    };    
   
    TimerTask task = new TimerTask() {    
        @Override    
        public void run() {    
            recLen--;    
            Message message = new Message();    
            message.what = 1;    
            handler.sendMessage(message);    
        }    
    };    
}
```

###方法三

* Handler与Message（不用TimerTask）  


```  
public class timerTask extends Activity{    
    private int recLen = 11;    
    private TextView txtView;    
   
    public void onCreate(Bundle savedInstanceState) {      
        super.onCreate(savedInstanceState);      
   
        setContentView(R.layout.timertask);     
        txtView = (TextView)findViewById(R.id.txttime);    
   
        Message message = handler.obtainMessage(1);     // Message    
        handler.sendMessageDelayed(message, 1000);    
    }      
   
    final Handler handler = new Handler(){    
   
        public void handleMessage(Message msg){         // handle message    
            switch (msg.what) {    
            case 1:    
                recLen--;    
                txtView.setText("" + recLen);    
   
                if(recLen > 0){    
                    Message message = handler.obtainMessage(1);    
                    handler.sendMessageDelayed(message, 1000);      // send message    
                }else{    
                    txtView.setVisibility(View.GONE);    
                }    
            }    
   
            super.handleMessage(msg);    
        }    
    };    
}
```


###方法四 

* Handler与Thread（不占用UI线程）

```
public class timerTask extends Activity{    
    private int recLen = 0;    
    private TextView txtView;    
   
    public void onCreate(Bundle savedInstanceState){    
        super.onCreate(savedInstanceState);    
   
        setContentView(R.layout.timertask);    
        txtView = (TextView)findViewById(R.id.txttime);    
            
        new Thread(new MyThread()).start();         // start thread    
    }       
   
    final Handler handler = new Handler(){          // handle    
        public void handleMessage(Message msg){    
            switch (msg.what) {    
            case 1:    
                recLen++;    
                txtView.setText("" + recLen);    
            }    
            super.handleMessage(msg);    
        }    
    };    
   
    public class MyThread implements Runnable{      // thread    
        @Override    
        public void run(){    
            while(true){    
                try{    
                    Thread.sleep(1000);     // sleep 1000ms    
                    Message message = new Message();    
                    message.what = 1;    
                    handler.sendMessage(message);    
                }catch (Exception e) {    
                
                }
            }
        }
    }
}
```

###方法五

* Handler与Runnable（最简单型）    

```
public class timerTask extends Activity{    
    private int recLen = 0;    
    private TextView txtView;    
   
    public void onCreate(Bundle savedInstanceState){    
        super.onCreate(savedInstanceState);    
   
        setContentView(R.layout.timertask);    
        txtView = (TextView)findViewById(R.id.txttime);    
            
        handler.postDelayed(runnable, 1000);    
    }       
   
    Handler handler = new Handler();    
    Runnable runnable = new Runnable() {    
        @Override    
        public void run() {    
            recLen++;    
            txtView.setText("" + recLen);    
            handler.postDelayed(this, 1000);    
        }    
    };    
} 
```

* 计时与倒计时 
  方法1，方法2和方法3，都是倒计时 
  方法4，方法5，都是计时 
  计时和倒计时，都可使用上述方法实现（代码稍加改动） 

* UI线程比较 
  方法1，方法2和方法3，都是在UI线程实现的计时； 
  方法4和方法5，是另开Runnable线程实现计时 

* 实现方式比较 
  方法1，采用的是Java实现，即Timer和TimerTask方式； 
  其它四种方法，都采用了Handler消息处理 

* 推荐使用 
  如果对UI线程交互要求不很高，可以选择方法2和方法3 
  如果考虑到UI线程阻塞，严重影响到用户体验，推荐使用方法4，另起线程单独用于计时和其它的逻辑处理 
  方法5，综合了前几种方法的优点，是最简的
