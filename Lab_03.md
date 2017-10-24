MAIN ACTIVITY
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

HANDLER

package fidelokoth.com.sqlitelab;

import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

import java.util.ArrayList;
import java.util.List;

import fidelokoth.com.sqlitelab.Contacts;

/**
 * Created by FIDEL OKOTH on 19/10/2017.
 */

public class DatabaseHandler extends SQLiteOpenHelper {

    //All Static variables
    //Database Version
    private static final int DATABASE_VERSION = 1;

    //Database Name
    private static final String DATABASE_NAME = "contactsManager";

    //Contact table name
    private static final String TABLE_CONTACTS = "contacts";

    //fidelokoth.com.sqlitelab.Contacts table columns anme
    private static final String KEY_ID = "id";
    private static final String KEY_NAME = "name";
    private static final String KEY_PH_NO = "phone_number";

    private static final String TABLE_FEABLE = "feable";

    private static final String KEY_UID = "uid";
    private static final String KEY_UNAME = "uname";
    private static final String KEY_UCODE = "ucode";


    public DatabaseHandler(Context context){
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }

    //Creating Tables
        @Override
    public void onCreate(SQLiteDatabase db){
        String CREATE_CONTACTS_TABLE = "CREATE TABLE " + TABLE_CONTACTS + "(" + KEY_ID + " INTEGER PRIMARY KEY, " + KEY_NAME + " TEXT, " + KEY_PH_NO + " TEXT" + ");";

        db.execSQL(CREATE_CONTACTS_TABLE);
    }

    //Upgrading database
    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {

        //Drop older table if existed
        db.execSQL("DROP TABLE IF EXISTS " + TABLE_CONTACTS);

        //Create tables again
        onCreate(db);
    }

    //CRUD operations

    //Adding new contactg
    public void addContacts(Contacts contacts) {
        SQLiteDatabase db = this.getWritableDatabase();

        ContentValues values = new ContentValues();
         values.put(KEY_NAME, contacts.getName());//contact name
        values.put(KEY_PH_NO, contacts.getphoneNumber()); //contact phone number

        //Inseerting Row
        db.insert(TABLE_CONTACTS, null, values);
        db.close(); //Closing database connection
    }
    public void addFeable(Feable unit) {
        SQLiteDatabase db = this.getWritableDatabase();

        ContentValues values = new ContentValues();
        values.put(KEY_UNAME, unit.get_uname());
        values.put(KEY_UCODE, unit.get_ucode());

        db.insert(TABLE_FEABLE, null, values);
        db.close();
    }



    //READING ROW(S)

    //Getting single contact
    public Contacts getcontacts(int id) {
        SQLiteDatabase db = this.getReadableDatabase();

        Cursor cursor = db.query(TABLE_CONTACTS, new String[] { KEY_ID, KEY_NAME, KEY_PH_NO}, KEY_ID + "=?",
                new String[]{String.valueOf(id)}, null, null, null, null);
        if(cursor != null)
            cursor.moveToFirst();

        Contacts contacts = new Contacts(Integer.parseInt(cursor.getString(0)),
                cursor.getString(1), cursor.getString(2));

        //return contact
        return contacts;
    }

    public Feable getFeable (int uid) {
        SQLiteDatabase db = this.getReadableDatabase();

        Cursor cursor = db.query(TABLE_FEABLE, new String[]{KEY_UID,
                        KEY_UNAME, KEY_UCODE}, KEY_UID + "=?",
                new String[]{String.valueOf(uid)}, null, null, null, null);

        if (cursor != null)
            cursor.moveToFirst();

        Feable feable = new Feable(Integer.parseInt(cursor.getString(0)),
                cursor.getString(1), cursor.getString(2));
        return feable;


    }


    //Getting All contacts
    public List<Contacts> getAllContacts() {
        List<Contacts> contactsList = new ArrayList<Contacts>();

        //Select All Query
        String selectquery = "SELECT * FROM " + TABLE_CONTACTS;

        SQLiteDatabase db = this.getWritableDatabase();
        Cursor cursor = db.rawQuery(selectquery, null);

        //looping through all rows and adding to list
        if(cursor.moveToFirst()){
            do{
                Contacts contacts = new Contacts ();
                contacts.setID(Integer.parseInt(cursor.getString(0)));
                contacts.setName(cursor.getString(1));
                Contacts.setPhoneNumber(cursor.getString(2));

                //Adding contact to list
                contactsList.add(contacts);
            }while (cursor.moveToNext());
        }
        //return contact list
        return contactsList;
    }

    public List<Feable> getAllFeable() {
        List<Feable> feableList = new ArrayList<>();
        //select All query
        String selectQuery = "SELECT  * FROM " + TABLE_FEABLE;
        SQLiteDatabase db = this.getWritableDatabase();
        Cursor cursor = db.rawQuery(selectQuery, null);
        //looping through all rows and adding to list
        if (cursor.moveToFirst()) {
            do {
                Feable feable = new Feable();
                feable.set_uid(Integer.parseInt(cursor.getString(0)));
                feable.set_uname(cursor.getString(1));
                feable.set_ucode(cursor.getString(2));
                //Adding contact to List
                feableList.add(feable);
            } while (cursor.moveToNext());
        }

        //return contact list
        return feableList;
    }


    //Getting contacts count
    public int getContactsCount() {
        String countQuery = "SELECT * FROM" + TABLE_CONTACTS;
        SQLiteDatabase db = this.getReadableDatabase();
        Cursor cursor = db.rawQuery(countQuery, null);
        cursor.close();

        //return count
        return cursor.getCount();
    }

    public int getFeableCount(){
        String countQuery;
        countQuery = "SELECT * FROM " +TABLE_FEABLE;
        SQLiteDatabase db = this.getReadableDatabase();
        Cursor cursor = db.rawQuery(countQuery,null);
        cursor.close();

        //return count
        return cursor.getCount();
    }


    //Updating single contact
    public int updateContact(Contacts contacts){
        SQLiteDatabase db = this.getWritableDatabase();

        ContentValues values = new ContentValues();
        values.put(KEY_NAME, contacts.getName());
        values.put(KEY_PH_NO, contacts.getphoneNumber());

        //updating row
        return db.update(TABLE_CONTACTS, values, KEY_ID + "= ?",
                new String[]{ String.valueOf(contacts.getID()) });
    }

    public int updateFeable(Feable feable) {
        SQLiteDatabase db = this.getWritableDatabase();

        ContentValues values = new ContentValues();
        values.put(KEY_UNAME, feable.get_uname() );
        values.put(KEY_UCODE, feable.get_ucode());

        return  db.update(TABLE_FEABLE, values,KEY_UID + "= ?",
                new String [] {String.valueOf(feable.get_uid())});
    }


    //deleting single contact
    public void deleteContacts(Contacts contacts) {
        SQLiteDatabase db = this.getWritableDatabase();
        db.delete(TABLE_CONTACTS, KEY_ID + "= ?",
                new String[]{ String.valueOf(contacts.getID()) });
        db.close();
    }

    public void deleteFeable(Feable feable){
        SQLiteDatabase db = this.getWritableDatabase();
        db.delete(TABLE_FEABLE,KEY_UID + " = ?",
                new String[] {String.valueOf(feable.get_uid())});
        db.close();
    }
}


JAVA CLASS

/**
 * Created by FIDEL OKOTH on 24/10/2017.
 */

public class Feable {
    //private variables
    int _uid;
    String _uname;
    String _ucode;
    //empty constructor
    public Feable(){

    }
    public Feable(int _uid, String _uname, String _ucode){
        this._uid =_uid;
        this._uname =_uname;
        this._ucode=_ucode;

    }

    public Feable(String _uname, String _ucode){
        this._uname = _uname;
        this._ucode = _ucode;
    }

    public int get_uid() {
        return _uid;
    }

    public void set_uid(int _id) {
        this._uid = _uid;
    }

    public String get_uname() {
        return _uname;
    }

    public void set_uname(String _uname) {
        this._uname = _uname;
    }

    public String get_ucode() {
        return _ucode;
    }

    public void set_ucode(String _ucode) {
        this._ucode = _ucode;
    }

}















