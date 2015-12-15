public class MainActivity extends Activity {

  //  @Override
    ListView t;
    EditText o,te;
    Button click,list;
    List<Contact> contacts;

    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
       t=(ListView)findViewById(R.id.txt);
        o=(EditText)findViewById(R.id.one);
       te=(EditText)findViewById(R.id.to);
        click=(Button)findViewById(R.id.BTN);
      final DatabaseHandler   db=new DatabaseHandler(this);
        click.setOnClickListener(new View.OnClickListener() {
            @Override

            public void onClick(View v) {
              if(  o.equals("")||te.equals("")){
                  Toast.makeText(getApplication(),"plse enter data",Toast.LENGTH_LONG).show();

              }
                else {
                  String fi = o.getText().toString();
                  String f = te.getText().toString();

                  db.addContact(new Contact(fi, f));
                  Toast.makeText(getApplication(), "data sent", Toast.LENGTH_LONG).show();
              }
            }
        });

        /**
         * CRUD Operations
         * */
        // Inserting Contacts

       list=(Button)findViewById(R.id.BTN1);



        list.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

             //db.getAllContacts();
                contacts =db.getAllContacts();

               DataAdapter adapter = new DataAdapter(MainActivity.this, R.layout.datalist, contacts);
                t.setAdapter(adapter);
            }
        });


        // Reading all contacts
        Log.d("Reading: ", "Reading all contacts..");


        t.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                final Contact ff=contacts.get(position);
               // final Integer delname=ff.getName();
                AlertDialog.Builder alertDialogBuilder = new AlertDialog.Builder(MainActivity.this);
                alertDialogBuilder.setMessage("Are you sure,You wanted to delete");

                alertDialogBuilder.setPositiveButton("yes", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface arg0, int arg1) {

                      db.deleteContact(ff);
                        t.getAdapter();
                    }
                });

                alertDialogBuilder.setNegativeButton("No",new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        finish();
                    }
                });

                AlertDialog alertDialog = alertDialogBuilder.create();
                alertDialog.show();
            }
        });

    }




}

