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



Covid_Enum</br>

package fxcollection;

public enum Covid_Enum {

    NEG(Integer.MIN_VALUE,-1),
    VERY_LOW(0,99),
    LOW(100,999),
    MEDIUM(1000,1999),
    HIGH(2000,4999),
    VERY_HIGH(5000,10000),
    OTHER(102071,Integer.MAX_VALUE);


    private final int min;
    private final int max;

    Covid_Enum(int min, int max) {
        this.min = min;
        this.max = max;
    }

    public int getMin() {
        return min;
    }

    public int getMax() {
        return max;
    }

}

 
 
 Categorizing</br>

package fxcollection;

import java.util.*;

public class Categorizing {
    public static Map<Covid_Enum, List<case_time_series_model>> getMapData(){

        Map<Covid_Enum,List<case_time_series_model>> totaldata = new HashMap<>();

        for(case_time_series_model model : ReadFromOPF.listof()){
            for(Covid_Enum dRecovered : Covid_Enum.values()){
                int recovered = Integer.parseInt(model.getDailyrecovered());
                //System.out.println(recovered);
                if(recovered>=dRecovered.getMin() && recovered<= dRecovered.getMax())
                    addtomap(dRecovered, model, totaldata);
            }

        }
        return  totaldata;
    }

    public static  void addtomap(Covid_Enum key, case_time_series_model model, Map<Covid_Enum, List<case_time_series_model>> map)
    {
        if(!map.containsKey(key)){
            map.put(key,new ArrayList<>(Arrays.asList(model)));
        }else{
            map.get(key).add(model);
        }
    }
}


Covid_Enum_Comparator</br>

package fxcollection;

import java.util.Comparator;

public class Covid_Enum_Comparator implements Comparator<Covid_Enum> {

    public Covid_Enum_Comparator() {
    }

    @Override
    public int compare(Covid_Enum o1, Covid_Enum o2) {
        if (o1.getMin() < o2.getMin())
            return -1;

        if (o1.getMin() > o2.getMin())
            return 1;
        else
            return 0;
    }





Javafx</br>
package fxcollection;

import API.Main;
import javafx.application.Application;
import javafx.beans.value.ChangeListener;
import javafx.beans.value.ObservableValue;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.scene.Scene;
import javafx.scene.control.ComboBox;
import javafx.scene.layout.BorderPane;
import javafx.stage.Stage;

import java.util.*;


public class Javafx extends Application {



        public static void main(String[] args) {
            launch(args);
        }

        @Override
        public void start(Stage primaryStage) {


            ComboBox<Covid_Enum> cov = new ComboBox<>();
            cov.setPromptText("--Select a Priority--");
            ComboBox<case_time_series_model> otherData = new ComboBox<>();
            otherData.setPromptText("Click here");
            otherData.setVisible(false);


            Map<Covid_Enum, List<case_time_series_model>> caswmap = Categorizing.getMapData();
            ObservableList<Covid_Enum> dataof = FXCollections.observableArrayList(caswmap.keySet());
            dataof.sort(new Covid_Enum_Comparator());
            cov.getItems().addAll(dataof);



            cov.valueProperty().addListener(new ChangeListener<Covid_Enum>() {
                @Override
                public void changed(ObservableValue<? extends Covid_Enum> observable, Covid_Enum oldValue, Covid_Enum newValue) {
                    otherData.getItems().clear();
                    otherData.getItems().addAll(caswmap.get(newValue));
                    otherData.setVisible(true);
                }
            });


            BorderPane pane = new BorderPane();
            pane.setTop(cov);
            pane.setCenter(otherData);
            Scene scene = new Scene(pane, 400, 400);
            primaryStage.setScene(scene);
            primaryStage.setTitle("Covid 19");
            primaryStage.show();

        }

    }


}



CaseTimeSeriesModel</br>

package fxcollection.models;

import java.util.Objects;

public class CaseTimeSeriesModel {


    private String date;
    private String dailyConfirmed;
    private String dailyDeceased;
    private String dailyRecovered;
    private String totalRecovered;
    private String totalConfirmed;
    private String totalDeceased;

    public CaseTimeSeriesModel(String dailyConfirmed, String dailyDeceased, String dailyRecovered, String totalRecovered, String totalConfirmed, String totalDeceased, String date) {

        this.dailyConfirmed = dailyConfirmed;
        this.dailyDeceased = dailyDeceased;
        this.dailyRecovered = dailyRecovered  ;
        this.totalRecovered = totalRecovered;
        this.totalConfirmed = totalConfirmed;
        this.totalDeceased = totalDeceased;
        this.date = date;
        //System.out.println(date);
    }

    @Override
    public String toString() {
        return "DATA OF COVID CASES { " + " date = " + date + " , dailyConfirmed = " + dailyConfirmed + " ,dailyDeceased =  " + dailyDeceased + ", dailyRecovered = " + dailyRecovered + "}";
    }


    public String getDate() {
        return date;
    }

    public void setDate(String date) {
        this.date = date;
    }

    public String getDailyConfirmed() {
        return dailyConfirmed;
    }

    public void setDailyConfirmed(String dailyConfirmed) {
        this.dailyConfirmed = dailyConfirmed;
    }

    public String getDailyDeceased() {
        return dailyDeceased;
    }

    public void setDailyDeceased(String dailyDeceased) {
        this.dailyDeceased = dailyDeceased;
    }

    public String getDailyRecovered() {
        return dailyRecovered;
    }

    public void setDailyRecovered(String dailyRecovered) {
        this.dailyRecovered = dailyRecovered;
    }

    public String getTotalRecovered() {
        return totalRecovered;
    }

    public void setTotalRecovered(String totalRecovered) {
        this.totalRecovered = totalRecovered;
    }

    public String getTotalConfirmed() {
        return totalConfirmed;
    }

    public void setTotalConfirmed(String totalConfirmed) {
        this.totalConfirmed = totalConfirmed;
    }

    public String getTotalDeceased() {
        return totalDeceased;
    }

    public void setTotalDeceased(String totalDeceased) {
        this.totalDeceased = totalDeceased;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        CaseTimeSeriesModel that = (CaseTimeSeriesModel) o;
        return Objects.equals(date, that.date) &&
                Objects.equals(dailyConfirmed, that.dailyConfirmed) &&
                Objects.equals(dailyDeceased, that.dailyDeceased) &&
                Objects.equals(dailyRecovered, that.dailyRecovered) &&
                Objects.equals(totalRecovered, that.totalRecovered) &&
                Objects.equals(totalConfirmed, that.totalConfirmed) &&
                Objects.equals(totalDeceased, that.totalDeceased);
    }

    @Override
    public int hashCode() {
        return Objects.hash(date, dailyConfirmed, dailyDeceased, dailyRecovered, totalRecovered, totalConfirmed, totalDeceased);
    }
}




Categorizing</br>

package fxcollection.models;

import java.io.IOException;
import java.net.URISyntaxException;
import java.util.*;

public class Categorizing {

    public static List<CaseTimeSeriesModel> getCovidInfo() throws IOException {
        return ReadFromOPF.listOf();
    }

    public static Map<CovidCasesRecord, List<CaseTimeSeriesModel>> getMapData() throws IOException, URISyntaxException {
        Map<CovidCasesRecord,List<CaseTimeSeriesModel>> totaldata = new HashMap<>();
        ReadFromOPF.listOf().forEach(model->placeValuesInDailyRecovered(totaldata,model));
        return  totaldata;
    }

    private static void placeValuesInDailyRecovered(Map<CovidCasesRecord, List<CaseTimeSeriesModel>> totaldata, CaseTimeSeriesModel model) {
        Arrays.asList(CovidCasesRecord.values()).forEach(dRecovered->{
            int recovered = Integer.parseInt(model.getDailyRecovered());
            if(recovered>=dRecovered.getMin() && recovered<= dRecovered.getMax())
               addtomap(dRecovered, model, totaldata);

        });
    }

    public static  void addtomap(CovidCasesRecord key, CaseTimeSeriesModel model, Map<CovidCasesRecord, List<CaseTimeSeriesModel>> map)
    {
        if(!map.containsKey(key)){
            map.put(key,new ArrayList<>(Arrays.asList(model)));
        }else{
            map.get(key).add(model);
        }
    }
}



CovidCasesRecord</br>

package fxcollection.models;

public enum CovidCasesRecord {

    NEG(Integer.MIN_VALUE,-1),
    VERY_LOW(0,99),
    LOW(100,999),
    MEDIUM(1000,1999),
    HIGH(2000,4999),
    VERY_HIGH(5000,10000),
    OTHER(102071,Integer.MAX_VALUE);


    private final int min;
    private final int max;

    CovidCasesRecord(int min, int max) {
        this.min = min;
        this.max = max;
    }

    public int getMin() {
        return min;
    }

    public int getMax() {
        return max;
    }

}




ReadFromOPF</br>

package fxcollection.models;

import java.io.BufferedReader;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.ArrayList;
import java.util.List;

public class ReadFromOPF {

    private static Path OPPath;
    private static String inputLine;

    public ReadFromOPF(Path OPPath) {
        this.OPPath = OPPath;
    }

    public static List<CaseTimeSeriesModel> listOf() throws IOException {
        List<CaseTimeSeriesModel> listofdata = new ArrayList<>();
        getPath();
        BufferedReader in = Files.newBufferedReader(OPPath);
        while ((inputLine = in.readLine()) != null) {
            StringBuilder stringBuilder = new StringBuilder();
            stringBuilder.append(inputLine+"\n");
            //System.out.println(inputLine);
            String[] data = stringBuilder.toString().split(" ");
            CaseTimeSeriesModel model = new CaseTimeSeriesModel(
                    data[0], data[1], data[2], data[3], data[4], data[5], data[6]+" "+data[7]
            );

            listofdata.add(model);
            //System.out.println(model);

        }


        return listofdata;
    }


    private static void getPath() throws IOException {
        OPPath = Path.of("build", "resources", "IOFile", "data.txt").toAbsolutePath();
        if (!Files.exists(OPPath)) {
            Files.createDirectories(OPPath);

        }
    }
}



CovidInfoDisplays</br>

package fxcollection.views;

import fxcollection.models.CaseTimeSeriesModel;
import fxcollection.models.CovidCasesRecord;
import static fxcollection.models.Categorizing.getCovidInfo;
import static fxcollection.models.Categorizing.getMapData;


import javafx.collections.ObservableList;
import javafx.scene.control.ComboBox;
import javafx.scene.control.ListCell;
import javafx.scene.text.Text;
import javafx.util.StringConverter;
import static javafx.collections.FXCollections.observableArrayList;

import java.io.IOException;
import java.net.URISyntaxException;
import java.util.List;
import java.util.Map;

public class CovidInfoDisplays {

    private ComboBox<CovidCasesRecord> priority;
    private ComboBox<CaseTimeSeriesModel> value;
    private final Text detailsTextBox;
    private final Map<CovidCasesRecord, List<CaseTimeSeriesModel>> caswmap;
    private final ObservableList<CovidCasesRecord> dataOf;

    public CovidInfoDisplays() throws IOException, URISyntaxException {
        caswmap =  getMapData();
        dataOf = observableArrayList(caswmap.keySet());
        detailsTextBox = new Text();
        setUpPrority();
        setUpValue();

    }

    private static class CovidStringConvertor extends StringConverter<CaseTimeSeriesModel> {
        private final String SEP = ", date ";

        @Override
        public String toString(CaseTimeSeriesModel caseTimeSeriesModel) {
            if (caseTimeSeriesModel == null)
                return null;

            else
                return "dailyRecovered:" + " " + caseTimeSeriesModel.getDailyRecovered() + SEP + caseTimeSeriesModel.getDate();
        }

        @Override
        public CaseTimeSeriesModel fromString(String string) {
            String date = string.split(SEP)[1];
            try {
                for (CaseTimeSeriesModel model : getCovidInfo()) {
                    if (model.getDate().equals(date))
                        return model;
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
            return null;
        }
    }


    private ObservableList<CovidCasesRecord> sortCases() {
            return dataOf.sorted((o1, o2) -> {
                if (o1.getMin() < o2.getMin())
                    return -1;

                if (o1.getMin() > o2.getMin())
                    return 1;
                else
                    return 0;
            });
        }


        private void setUpPrority() {
            priority = new ComboBox<>();
            priority.getItems().addAll(sortCases());
            priority.setPromptText("--Select a Priority--");
            priority.valueProperty().addListener((observable, oldValue, newValue) -> {
                detailsTextBox.setVisible(false);
                value.getItems().clear();
                value.getItems().addAll(caswmap.get(newValue));
                value.setVisible(true);

            });
        }

        private void setUpValue(){
     value = new ComboBox<>();
     value.setPromptText("Click here");
     value.setConverter(new CovidStringConvertor());
     createDataSelectionListener();
     value.setVisible(false);
     handleValueComboBoxUpdate();

    }

    private void handleValueComboBoxUpdate(){
        value.setButtonCell(new ListCell<>(){
            @Override
            protected void updateItem(CaseTimeSeriesModel caseTimeSeriesModel,boolean empty){
                super.updateItem(caseTimeSeriesModel,empty);
                        if(empty || caseTimeSeriesModel == null)
                            setText("Click here");
                        else{
                            CovidStringConvertor convertor = new CovidStringConvertor();
                        setText(convertor.toString(caseTimeSeriesModel));
            }
        }
    });
    }

    private void createDataSelectionListener(){
        value.valueProperty().addListener( (observable, oldValue, newValue) -> {
            if(newValue!=null){
                String displayText="date:"+newValue.getDate() + "\n"
                +"dailyConfirmed:"+newValue.getDailyConfirmed()+"\n"
                        +"dailyDeceased:"+newValue.getDailyDeceased()+"\n"
                        +"dailyRecovered:"+newValue.getDailyRecovered()+"\n";
                detailsTextBox.setText(displayText);
                detailsTextBox.setVisible(true);
            }
        });

    }

    public ComboBox<CovidCasesRecord> getPriority() {
        return priority;
    }

    public ComboBox<CaseTimeSeriesModel> getValue() {
        return value;
    }

    public Text getDetailsTextBox() {
        return detailsTextBox;
    }
}



CovidApp</br>

package fxcollection;

import fxcollection.models.CaseTimeSeriesModel;
import fxcollection.models.CovidCasesRecord;
import fxcollection.views.CovidInfoDisplays;

import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.scene.Scene;
import javafx.scene.control.ComboBox;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.HBox;
import javafx.scene.text.Text;
import javafx.stage.Stage;

import java.io.IOException;
import java.net.URISyntaxException;


public class CovidApp extends Application {
         private CovidInfoDisplays comboBoxes;

         public static void main(String[] args) {
            launch(args);
        }

        @Override
        public void start(Stage primaryStage) throws IOException, URISyntaxException {
            comboBoxes = new CovidInfoDisplays();
            BorderPane borderPane = new BorderPane();
            setUpBorderPane(borderPane);
            Scene scene = new Scene(borderPane, 600, 200);
            primaryStage.setScene(scene);
            primaryStage.setTitle("Covid 19");
            primaryStage.show();

        }

        private void setUpBorderPane(BorderPane borderPane){
            HBox horBox= new HBox();
            setUpHBox(horBox);
            borderPane.setTop(horBox);

         }

       private void setUpHBox(HBox horBox) {
        horBox.setSpacing(10);
        ComboBox<CovidCasesRecord> covFirst = comboBoxes.getPriority();
        ComboBox<CaseTimeSeriesModel> covSecond = comboBoxes.getValue();
        Text detailsTextBox = comboBoxes.getDetailsTextBox();
        horBox.getChildren().addAll(covFirst,covSecond,detailsTextBox);
        horBox.setMargin(covFirst,new Insets(20,5,5,10));
        horBox.setMargin(covSecond,new Insets(20, 5, 5,5));
        horBox.setMargin(detailsTextBox,new Insets( 20,5,5,5));
    }

}



DuplicateApp</br>

package duplicates;

import java.time.Duration;
import java.time.Instant;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.function.Predicate;

public class DuplicateApp {
    public DuplicateApp() {
    }

    public static void main(String[] args) {
       List<Integer> numList = randomIntegers();
        timeDuplicateMethod("Loops",list->DuplicateUtil.isUniqueLoopsCheck(numList),numList);
        timeDuplicateMethod("Set",list->DuplicateUtil.isUniqueSetCheck(numList),numList);
        timeDuplicateMethod("Map",list->DuplicateUtil.isUniqueMapCheck(numList),numList);


    }

        public static void timeDuplicateMethod (String Mname, Predicate <List> predicate,List<Integer> list) {

            System.out.println("Method:"+Mname);
            Instant start = Instant.now();
            boolean result =predicate.test(list);
            System.out.println("Result:" +result);
            Instant stop = Instant.now();
            long timeElapsed = Duration.between(start, stop).toNanos();
            System.out.println("Time in nanoseconds:"+timeElapsed);
    }

    public static List<Integer> randomIntegers () {
        List<Integer> randomList = new ArrayList<>();
        Random random = new Random();
        int low = 0;
        int high = 15000;
        int count = 0;
        for (int i = 0; i < 10000; i++) {
            int result = (int) (Math.random() * (high - low)) + low;
            count++;

            //System.out.println(result);

            randomList.add(result);

        }
        //System.out.println(randomList);
       //System.out.println(count);

        return randomList;
    }

}




DuplicateUtil</br>

package duplicates;

import java.util.*;

public class DuplicateUtil {
    private DuplicateUtil() {
    }

    public static boolean isUniqueLoopsCheck(List<Integer> list) {
        List<Integer> listOfIntegers = new ArrayList<>();

        for (int i = 0; i < list.size(); i++) {
            for (int j = i + 1; j < list.size(); j++) {

                if ((list.get(i) == list.get(j)) && i != j) {
                    //System.out.println("false");
                    return false;
                }

            }

        }
        return true;
    }

    public static boolean isUniqueSetCheck(List<Integer> list) {
        Set<Integer> hashSet = new HashSet<>();

        for (int i : list) {
            if (!hashSet.add(i)) {

                return false;
            }
        }
        //System.out.println("hs"+hashSet);
        return true;
    }


    public static boolean isUniqueMapCheck(List<Integer> list) {
        List<Integer> numList = new ArrayList<>();
        Map<Integer, List<Integer>> hashMap = new HashMap<>();

        for (int i : list) {
            if (hashMap.containsKey(i)) {

                return false;


            } else{
                hashMap.put(i, numList);
            }

        }
            return true;

        }
}





DuplicateUtilTest</br>

package duplicates;

import org.junit.jupiter.api.Test;

import java.lang.reflect.Constructor;
import java.lang.reflect.Modifier;
import java.util.Arrays;
import java.util.List;

import static org.junit.jupiter.api.Assertions.*;

class DuplicateUtilTest {

    @Test
    void shouldTestPrivateConstructor() throws Exception {
        Constructor<DuplicateUtil> constructor = DuplicateUtil.class.getDeclaredConstructor();
        assertTrue(Modifier.isPrivate(constructor.getModifiers()));
    }

    @Test
    void shouldReturnTrueForAllUniqueIsUniqueLoopsCheck() {
        List<Integer> numbers = Arrays.asList(9, 2, 3, -2);
        assertTrue(DuplicateUtil.isUniqueLoopsCheck(numbers));
    }

    @Test
    void shouldReturnFalseForDupsIsUniqueLoopsCheck() {
        List<Integer> numbers = Arrays.asList(9, 2, 3, 2);
        assertFalse(DuplicateUtil.isUniqueLoopsCheck(numbers));
    }

    @Test
    void shouldReturnTrueForAllUniqueIsUniqueSetCheck() {
        List<Integer> numbers = Arrays.asList(9, 2, 3, -2);
        assertTrue(DuplicateUtil.isUniqueSetCheck(numbers));
    }

    @Test
    void shouldReturnFalseForDupsIsUniqueSetCheck() {
        List<Integer> numbers = Arrays.asList(9, 2, 3, 2);
        assertFalse(DuplicateUtil.isUniqueSetCheck(numbers));
    }

    @Test
    void shouldReturnTrueAllUniqueIsUniqueMapCheck() {
        List<Integer> numbers = Arrays.asList(9, 2, 3, -2);
        assertTrue(DuplicateUtil.isUniqueMapCheck(numbers));
    }

    @Test
    void shouldReturnFalseForDupsIsUniqueMapCheck() {
        List<Integer> numbers = Arrays.asList(9, 2, 3, 2);
        assertFalse(DuplicateUtil.isUniqueMapCheck(numbers));
    }
}

