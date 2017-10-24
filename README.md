package fidelokoth.com.sqlitelab;

import android.os.Bundle;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.Snackbar;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.util.Log;
import android.view.View;
import android.view.Menu;
import android.view.MenuItem;

import java.util.List;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        DatabaseHandler db = new DatabaseHandler(this);

        /**
         * CRUD Operations
         */
        //Inserting fidelokoth.com.sqlitelab.Contacts
        Log.d("Insert: ","Inserting ..");
        db.addContacts(new Contacts("Ravi", "9100000000"));
        db.addContacts(new Contacts("Srinivas","9199999999"));
        db.addContacts(new Contacts("Tommy","9522222222"));
        db.addContacts(new Contacts("Karthik", "9533333333"));

        //Reading all contacts
        Log.d("Reading: ", "Reading all contacts..");
        List<Contacts> contacts = db.getAllContacts();

        for(Contacts cn : contacts) {
            String log = "Id: " + cn.getID() + " ,Name: " + cn.getName() + " ,phone: " + cn.getphoneNumber();
            Log.d("Name: ", log);
        }

    }



        Log.("Insert:","Inserting..");
                db.addFeable(new Feable(" pop"," 678383873789"));
                db.addFeable(new Feable(" reggae"," 3443494948494"));
                db.addFeable(new Feable(" hiphop"," 2883938383983"));
                db.addFeable(new Feable(" rock"," 723939273932938392"));
                //reading all contacts
                Log.d("Reading:","Reading all contacts..");
                List<Feable> feable = db.getAllFeable();

        for (Feable un : feable) {
        String log = "Uid:" + un.get_uid() + ",Name:" + un.get_uname() + ",Phone:" + un.get_ucode();
        // writing contacts to log
        Log.i("Name:", log);
        }


        //FloatingActionButton fab = (FloatingActionButton) findViewById(R.id.fab);
        //fab.setOnClickListener(new View.OnClickListener() {
        //  @Override
        //public void onClick(View view) {
        //  Snackbar.make(view, "Replace with your own action", Snackbar.LENGTH_LONG)
        //        .setAction("Action", null).show();
        // }
        //});


@Override
public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
        }

@Override
public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
        return true;
        }

        return super.onOptionsItemSelected(item);
        }
}
