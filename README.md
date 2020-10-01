# neiu-cs-420
                                      # Object Oriented Design
                                      # Project Proposal


# Dataset 
API 
https://api.covid19india.org/

# Description
This API is an open source API produced by Indian government to track data on COVID-19. It has all the information regarding COVID-19 in India, so anyone can get the required data according to their suitability. It does not require API key.</br>
It includes:</br>

o	State Level : it contains district information	</br>
o	State Level : it contains information about daily changes</br>
o	State Level : it contains data of the tests undergone	</br>
o	National/State Level: Time series for checking symptoms	</br>
o	District Level : It contains information of changes in districts etc.</br>

# Project name
COVID-19 data analysis for India.

# Project objective:

COVID-19 is an infectious disease which is spreading very fast globally, leaving millions of lives at stake. Currently, transparency is maintained regarding the information of infected cases, recovered cases, deaths but projections and graphs are not that clear so more concentration will be laid on showing graphical representations.</br>
The aim of this project is to analyze the data of a particular country, taking each and every state and district into consideration so that maximum and minimum effected places can be figured out easily. And there will be graphical presentation of the records which would help people in better understanding the situation of their country, state and district. These graphs would be beneficial in bringing awareness among people so as to how important it is to follow precautions and take extra measures to stay safe and save other’s lives too.</br></br>
What is the death rate in india due to COVID-19?</br>
How many COVID-19 confirmed cases in India?</br>
What is the current COVID-19 situation in a country,state, district?</br>

# Motivation

What is COVID-19?<br>
Coronavirus belongs to a huge group of viruses which causes infection in humans and animals. In humans, few coronaviruses are responsible in causing respiratory infections which includes common cold, Middle East Respiratory Syndrome (MERS) and Severe Acute Respiratory Syndrome (SARS).The most lately discovered coronavirus is COVID-19. It is a contagious and air borne disease, which was outbroke in Wuhan city, China in 2019. It took less than six months to spread globally and now it’s a pandemic.</br>

As it is impacting entire world it caught my interest to contribute in this regard.So I am choosing a particular country India and analysing the data to show some accurate information of coronavirus cases, because India has a largest population around the world. In addition, project includes graphical presentations.


                                                             #Retrieveing Information from API 

IOFile</br>
package API;

import java.io.*;

public class IOFile implements IOFileImp {

           @Override
           public File openFile(String fileName)
           {

              File file = null;
                 try {
                    String path = ClassLoader.getSystemClassLoader().getResource("").toURI().getPath();
                    path = path.substring(0, path.indexOf("classes"))+"resources" + File.separator + "IOFile"+File.separator;
                    System.out.println(path);
                    File folder = new File(path + "" + File.separator);      
                    if (!folder.exists()) {                                           
                        folder.mkdirs();                                               
                       }                                                                
                    file = new File(folder + File.separator + fileName);
                    if (!file.exists()) {
                        file.createNewFile();
                       }
                    }catch (Exception e) {
                         System.out.println(e.toString());
                       }
                    return file;
           }


            @Override
            public String readTextFromFile(File file)

            {
                try
                {
                    int i;
                    char c;
                    FileInputStream fis = new FileInputStream(file);
                    while((i = fis.read())!=-1) {
                         c = (char)i;
                    }

                } catch (FileNotFoundException fnf) {
                    System.out.println("No File");
                }catch (IOException e ){
                    e.printStackTrace();
                }

                return null;
            }
            @Override
            public void writeTextOnFile(String content,String fileName)
            {
                File file=openFile(fileName);
                try  {
                    FileOutputStream  out= new FileOutputStream(file);
                     out.write(content.getBytes());
                     out.close();
                } catch (FileNotFoundException fnf) {
                    System.out.println("No File");
                } catch (IOException e){
                    e.printStackTrace();
                }


            }

}</br>



IOFileImp

import java.io.File;

public interface IOFileImp {</br>
         File openFile(String fileName);</br>
         String readTextFromFile(File file);</br>
         void writeTextOnFile(String content,String file);</br>
    }</br>



JSONReader</br>
package API;</br>
import org.json.simple.JSONArray;</br>
import org.json.simple.JSONObject;</br>
import org.json.simple.parser.JSONParser;</br>
import org.json.simple.parser.ParseException;</br>

import java.io.BufferedReader;</br>
import java.io.InputStreamReader;</br>
import java.net.URL;</br>
import java.net.URLConnection;</br>

public class JSONReader implements JSONReaderImp {</br>

    @Override
    public void parseJSON(String data, String fileName)
        {
            String jsonData = "";
        try{
            JSONParser parser = new JSONParser();</br>
            JSONObject obj = (JSONObject) parser.parse(data);
            JSONArray national = (JSONArray) obj.get("cases_time_series");


            for(int i=0;i<=100;i++)
            {
                JSONObject jsonObject=(JSONObject)national.get(i);
                jsonData+=("\n-------------------- ELEMENT "+i+" --------------------------");
                jsonData+=("\ndailyconfirmed:"+jsonObject.get("dailyconfirmed"));
                jsonData+=("\ndailydeceased:"+jsonObject.get("dailydeceased"));
                jsonData+=("\ndailyrecovered:"+jsonObject.get("dailyrecovered"));
                jsonData+=("\ndate:"+jsonObject.get("date"));
                jsonData+=("\ntotalconfirmed:"+jsonObject.get("totalconfirmed"));
                jsonData+=("\ntotaldeceased:"+jsonObject.get("totaldeceased"));
                jsonData+=("\ntotalrecovered:"+jsonObject.get("totalrecovered"));
            }

        }catch (ParseException e)

        {
            System.out.println(e.toString());
        }

            System.out.println(jsonData);
            IOFile ioFile = new IOFile();
            ioFile.writeTextOnFile(jsonData,fileName);
        }

        @Override
        public String readDataFromURL(String api) {
            StringBuilder stringBuilder = new StringBuilder();
            try {
                URL link = new URL(api);
                URLConnection yc = link.openConnection();
                BufferedReader in = new BufferedReader(new InputStreamReader(yc.getInputStream()));
                String inputLine;
                while ((inputLine = in.readLine()) != null)
                    stringBuilder.append(inputLine + "\n");
                    in.close();
                }catch (Exception e){
                e.printStackTrace();
            }
                return stringBuilder.toString();

}

}</br>



JSONReaderImp</br>
package API;

public interface JSONReaderImp {

    void parseJSON(String data, String fileName);
    String readDataFromURL(String api);
}</br>




Main</br>
package API;


import java.io.*;

public class Main {

    public static final String apiURL="https://api.covid19india.org/data.json";

        public static final String fileName="data.txt";

        public static void main(String[] args) throws IOException {

            JSONReader jsonReader=new JSONReader();

           

            String data=jsonReader.readDataFromURL(apiURL);



            jsonReader.parseJSON(data,fileName);



        }
    }




fxcollection</br>


ReadFromOPF class</br>

package fxcollection;

import java.io.File;
import java.io.IOException;
import java.net.URISyntaxException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class ReadFromOPF {

    public static List<case_time_series_model> listof (){
        List<case_time_series_model> listofdata = new ArrayList<>();
        try {
            File file = getFile();

            //System.out.println("dkdsjdskdj " + file.toString());
            Scanner scanner = new Scanner(new File(file.toString()));

            while (scanner.hasNext()) {
                String line = scanner.nextLine();
                String[] data = line.split(" ");


                case_time_series_model model = new case_time_series_model(
                        data[0], data[1], data[2], data[3], data[4], data[5], data[6]
                );

                listofdata.add(model);

            }
            scanner.close();

        } catch (Exception e) {
            System.out.println("File not found!");
        }

        return listofdata;

    }


    public static File getFile() throws URISyntaxException {
        File file =null;
        String path = ClassLoader.getSystemClassLoader().getResource("").toURI().getPath();
        path = path.substring(0, path.indexOf("classes"))+"resources" + File.separator + "IOFile"+File.separator;

        File folder = new File(path + "" + File.separator);
        file = new File(folder + File.separator + "data.txt");
        return file;
    }
}



case_time_series_model</br> 

package fxcollection;

import java.io.File;
import java.util.*;

public class case_time_series_model {


    private String date;
    private String dailyconfirmed;
    private String dailydeceased;
    private String dailyrecovered;
    private String totalrecovered;
    private String totalconfirmed;
    private String totaldeceased;

    public case_time_series_model(String dailyconfirmed, String dailydeceased, String dailyrecovered, String totalrecovered, String totalconfirmed, String totaldeceased, String date) {

        this.dailyconfirmed = dailyconfirmed;
        this.dailydeceased = dailydeceased;
        this.dailyrecovered = dailyrecovered;
        this.totalrecovered = totalrecovered;
        this.totalconfirmed = totalconfirmed;
        this.totaldeceased = totaldeceased;
        this.date = date;
    }

    @Override
    public String toString() {
        return "DATA OF { " + " date = " + date + " , dailyconfirmed = " + dailyconfirmed + " ,dailydeceased =  " + dailydeceased + ", dailyrecovered = " + dailyrecovered + "}";
    }


    public String getDate() {
        return date;
    }

    public void setDate(String date) {
        this.date = date;
    }

    public String getDailyconfirmed() {
        return dailyconfirmed;
    }

    public void setDailyconfirmed(String dailyconfirmed) {
        this.dailyconfirmed = dailyconfirmed;
    }

    public String getDailydeceased() {
        return dailydeceased;
    }

    public void setDailydeceased(String dailydeceased) {
        this.dailydeceased = dailydeceased;
    }

    public String getDailyrecovered() {
        return dailyrecovered;
    }

    public void setDailyrecovered(String dailyrecovered) {
        this.dailyrecovered = dailyrecovered;
    }

    public String getTotalrecovered() {
        return totalrecovered;
    }

    public void setTotalrecovered(String totalrecovered) {
        this.totalrecovered = totalrecovered;
    }

    public String getTotalconfirmed() {
        return totalconfirmed;
    }

    public void setTotalconfirmed(String totalconfirmed) {
        this.totalconfirmed = totalconfirmed;
    }

    public String getTotaldeceased() {
        return totaldeceased;
    }

    public void setTotaldeceased(String totaldeceased) {
        this.totaldeceased = totaldeceased;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        case_time_series_model that = (case_time_series_model) o;
        return Objects.equals(date, that.date) &&
                Objects.equals(dailyconfirmed, that.dailyconfirmed) &&
                Objects.equals(dailydeceased, that.dailydeceased) &&
                Objects.equals(dailyrecovered, that.dailyrecovered) &&
                Objects.equals(totalrecovered, that.totalrecovered) &&
                Objects.equals(totalconfirmed, that.totalconfirmed) &&
                Objects.equals(totaldeceased, that.totaldeceased);
    }

    @Override
    public int hashCode() {
        return Objects.hash(date, dailyconfirmed, dailydeceased, dailyrecovered, totalrecovered, totalconfirmed, totaldeceased);
    }
}




Categorizing</br>

package fxcollection;

import java.util.*;

public class Categorizing {
    public static Map<String , List<case_time_series_model>> getMapData(){

        Map<String ,List<case_time_series_model>> totaldata = new HashMap<>();


        for(case_time_series_model model : ReadFromOPF.listof()){
            if(Integer.parseInt(model.getDailyrecovered())<100){
                addtomap("Very low",model,totaldata);

            } else if(Integer.parseInt(model.getDailyrecovered())<1000){
                addtomap("Low",model,totaldata);

            }else if(Integer.parseInt(model.getDailyrecovered())<2000){
                addtomap("Medium",model,totaldata);

            }else if(Integer.parseInt(model.getDailyrecovered())<5000){
                addtomap("High",model,totaldata);

            }else if(Integer.parseInt(model.getDailyrecovered())<10000){
                addtomap("Very high",model,totaldata);

            }
        }
        return  totaldata;
    }

    public static  void addtomap(String key,case_time_series_model model,Map<String,List<case_time_series_model>> map)
    {
        if(!map.containsKey(key)){
            map.put(key,new ArrayList<>(Arrays.asList(model)));
        }else{
            map.get(key).add(model);
        }
    }
}


javafx</br>

package fxcollection;

import javafx.application.Application;
import javafx.beans.value.ChangeListener;
import javafx.beans.value.ObservableValue;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.scene.Scene;
import javafx.scene.control.ComboBox;
import javafx.scene.layout.BorderPane;
import javafx.stage.Stage;
import API.Main;
import java.io.File;
import java.io.FileNotFoundException;
import java.util.*;


public class Javafx extends Application {



        public static void main(String[] args) {
            launch(args);
        }

        @Override
        public void start(Stage primaryStage) {


            ComboBox<String> date = new ComboBox<>();
            date.setPromptText("--Select a Priority--");
            ComboBox<case_time_series_model> otherData = new ComboBox<>();
            otherData.setPromptText("Click here");
            otherData.setVisible(false);

            Map<String, List<case_time_series_model>> caswmap = Categorizing.getMapData();
            ObservableList<String> dataof = FXCollections.observableArrayList(caswmap.keySet());
            date.getItems().addAll(dataof);

            date.valueProperty().addListener(new ChangeListener<String>() {
                @Override
                public void changed(ObservableValue<? extends String> observable, String oldValue, String newValue) {
                    otherData.getItems().clear();
                    otherData.getItems().addAll(caswmap.get(newValue));
                    otherData.setVisible(true);
                }
            });


            BorderPane pane = new BorderPane();
            pane.setTop(date);
            pane.setCenter(otherData);
            Scene scene = new Scene(pane, 400, 400);
            primaryStage.setScene(scene);
            primaryStage.setTitle("Covid 19");
            primaryStage.show();

        }

    }




