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

API PACKAGE</br>
JSONReader</br>


package api;
import org.json.JSONArray;
import org.json.JSONObject;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

import java.net.URL;
import java.net.URLConnection;

import java.util.stream.Collectors;
import java.util.stream.Stream;
import java.util.stream.StreamSupport;

public class JSONReader implements JSONReaderImp   {

   private BufferedReader bufferedReader;
   private String data,dataStringOne,dataStringTwo;
   private RecordData recordData;

   public JSONReader(String api) throws IOException {

        URL link = new URL(api);
        URLConnection yc = link.openConnection();
        bufferedReader = new BufferedReader(new InputStreamReader(yc.getInputStream()));
        this.recordData = new RecordData();
    }

    @Override
    public Stream<Object> arrayToStream(JSONArray array) {
        return StreamSupport.stream(array.spliterator(), false);
    }


    @Override
   public void apiNationalData(String fileName)  {

        data = bufferedReader.lines().collect(Collectors.joining());
        JSONArray jsonArrayNational = (new JSONObject(data)).getJSONArray("cases_time_series");
        dataStringOne = getStringNational(jsonArrayNational);
        recordData.recordNationalData(dataStringOne,fileName);
   }

    @Override
    public void apiStateData(String fileNameNew) {

        JSONArray jsonArrayState = (new JSONObject(data)).getJSONArray("statewise");
        dataStringTwo = getStringState(jsonArrayState);
        recordData.recordStateData(dataStringTwo, fileNameNew);

   }

   @Override
   public String getStringNational(JSONArray jsonArray) {
                 dataStringOne = arrayToStream(jsonArray)
                .map(JSONObject.class::cast)
                .map(jsonObject -> ""

                        .concat((String) jsonObject.get("dailyconfirmed"))
                        .concat(" " + jsonObject.get("dailydeceased"))
                        .concat(" " + jsonObject.get("dailyrecovered"))
                        .concat(" " + jsonObject.get("totalrecovered"))
                        .concat(" " + jsonObject.get("totalconfirmed"))
                        .concat(" " + jsonObject.get("totaldeceased"))
                        .concat(" " + jsonObject.get("date")))
                .collect(Collectors.joining("\n"));
        return dataStringOne;
    }

    @Override
    public String getStringState(JSONArray jsonArray) {
                 dataStringTwo = arrayToStream(jsonArray)
                .map(JSONObject.class::cast)
                .map(jsonObject -> ""

                        .concat((String) jsonObject.get("active"))
                        .concat(" " + jsonObject.get("confirmed"))
                        .concat(" " + jsonObject.get("deaths"))
                        .concat(" " + jsonObject.get("deltaconfirmed"))
                        .concat(" " + jsonObject.get("deltadeaths"))
                        .concat(" " + jsonObject.get("deltarecovered"))
                        .concat(" " + jsonObject.get("migratedother"))
                        .concat(" " + jsonObject.get("recovered"))
                        .concat(" " + jsonObject.get("statecode")))

                .collect(Collectors.joining("\n"));
        return dataStringTwo;
    }
}


JSONReaderImp</br>

package api;

import org.json.JSONArray;

import java.io.IOException;
import java.net.URISyntaxException;
import java.util.stream.Stream;

public interface JSONReaderImp {


    Stream<Object> arrayToStream(JSONArray array);

    void apiNationalData(String fileName) throws IOException;

    void apiStateData(String fileNameNew) throws IOException, URISyntaxException;

    String getStringNational(JSONArray jsonArray);

    String getStringState(JSONArray jsonArray);
}


RecordData</br>

package api;

import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.OutputStream;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

public class RecordData implements RecordDataImp {

  private final Path opPath1;
  private final Path opPath2;

    public RecordData() {
        this.opPath1 =Path.of( "build", "resources", "IOFile", "Nationaldata.txt").toAbsolutePath();
        this.opPath2 = Path.of("build", "resources", "IOFile", "Statedata.txt").toAbsolutePath();
    }

    @Override
    public void recordNationalData(String content, String fileName){
        try {
            createDirectory();
            OutputStream national =  Files.newOutputStream(opPath1);
            national.write(content.getBytes());
            national.close();
        } catch (FileNotFoundException fnf) {
            System.out.println("No File");
        } catch (IOException e) {
            e.printStackTrace();

        }

    }

    @Override
    public void recordStateData(String contentNew, String fileNameNew) {
        try {
            createDirectory();
            OutputStream state = Files.newOutputStream(opPath2);
            state.write(contentNew.getBytes());
            state.close();
        } catch (FileNotFoundException fnf) {
            System.out.println("No File");
        } catch (IOException e) {
            e.printStackTrace();

        }

    }

        private void createDirectory() throws IOException {
        Path opPath = Paths.get("build", "resources", "IOFile").toAbsolutePath();
        if (!Files.exists(opPath)) {
            Files.createDirectories(opPath);
        }
    }
}



RecordDataImp</br>

package api;


import java.io.IOException;
import java.net.URISyntaxException;


public interface RecordDataImp {

    void recordNationalData(String content, String file) throws IOException, URISyntaxException;
    void recordStateData(String content, String file) throws IOException, URISyntaxException;
    }



Package models</br>

Categorizing</br>

package fxcollection.models;

import java.util.*;

public class Categorizing {

    private ReadAndStoreData read;

    public Categorizing() {
        this.read = new ReadAndStoreData();
    }

    public Categorizing(ReadAndStoreData read) {
        this.read = read;
    }

    public List<NationalInformation> getCovidInfo()  {
        return read.getListOfNationalData();
    }
    public Map<CovidCasesNationDisplay, List<NationalInformation>> getMapData() {

        Map<CovidCasesNationDisplay,List<NationalInformation>> totalData = new HashMap<>();
        read.getListOfNationalData().stream().forEach(model->placeValuesInDailyRecovered(totalData,model));
        return  totalData;
    }

    private void placeValuesInDailyRecovered(Map<CovidCasesNationDisplay, List<NationalInformation>> totalData, NationalInformation model) {
        Arrays.asList(CovidCasesNationDisplay.values()).stream().forEach(dailyConfirmed->{
            String month = model.getDate().split(" ")[1];
            if(month.equalsIgnoreCase(String.valueOf(dailyConfirmed)))
                addToMap(dailyConfirmed, model, totalData);

        });
    }


   private void addToMap(CovidCasesNationDisplay key, NationalInformation model, Map<CovidCasesNationDisplay, List<NationalInformation>> map)
    {
        if(!map.containsKey(key)){
            map.put(key,new ArrayList<>(Arrays.asList(model)));
        }else{
            map.get(key).add(model);
        }
    }
}


CovidCasesNationalDisplay</br>

package fxcollection.models;

public enum CovidCasesNationDisplay {

    JANUARY(0,1),
    FEBRUARY(0,2),
    MARCH(0,3),
    APRIL(0,4),
    MAY(0,5),
    JUNE(0,6),
    JULY(0,7),
    AUGUST(0,8),
    SEPTEMBER(0,9),
    OCTOBER(0,10),
    NOVEMBER(0,11),
    DECEMBER(0,12);

    private final int min;
    private final int max;

    CovidCasesNationDisplay(int min, int max) {
        this.min = min;
        this.max = max;
    }

    public int getMax() {
        return max;
    }

}



NationalInformation</br>

package fxcollection.models;

import java.util.Objects;

public class NationalInformation {


    private String date;
    private String dailyConfirmed;
    private String dailyDeceased;
    private String dailyRecovered;
    private String totalRecovered;
    private String totalConfirmed;
    private String totalDeceased;

    public NationalInformation(String dailyConfirmed, String dailyDeceased, String dailyRecovered, String totalRecovered, String totalConfirmed, String totalDeceased, String date) {

        this.dailyConfirmed = dailyConfirmed;
        this.dailyDeceased =  dailyDeceased;
        this.dailyRecovered = dailyRecovered  ;
        this.totalRecovered = totalRecovered;
        this.totalConfirmed = totalConfirmed;
        this.totalDeceased =  totalDeceased;
        this.date = date;
    }

    @Override
    public String toString() {
        return "DATA OF COVID CASES { " + " date = " + date + " , daily confirmed = " + dailyConfirmed + " ,daily deceased =  " + dailyDeceased + ", daily recovered = " + dailyRecovered + ",total confirmed="+totalConfirmed+",total deceased ="+totalDeceased+",total recovered="+totalRecovered;
    }


    public String getDate()
    { return date;

    }

    public String getDailyConfirmed() {
        return dailyConfirmed;
    }

    public String getDailyDeceased() {
        return dailyDeceased;
    }

    public String getDailyRecovered() {
        return dailyRecovered;
    }

    public String getTotalRecovered() {
        return totalRecovered;
    }

    public String getTotalConfirmed() {
        return totalConfirmed;
    }

    public String getTotalDeceased() {
        return totalDeceased;
    }



    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        NationalInformation that = (NationalInformation) o;
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



ReadAndStoreData</br>

package fxcollection.models;

import api.JSONReader;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.HBox;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class ReadAndStoreData {

    private Path OPPath;
    private List<NationalInformation> listOfNationalData;
    private List<StateInformation> listOfStateData;
    private JSONReader jsonReader;
    private BorderPane pane;
    private HBox hbox;
    private boolean show;

    public boolean isShow() {
        return show;
    }
    public ReadAndStoreData(){

        pane = new BorderPane();
        hbox = new HBox();

        try{
        OPPath = getPath();
        final String apiURL = "https://api.covid19india.org/data.json";
        jsonReader = new JSONReader(apiURL);

        String fileName = "Nationaldata.txt";
        if (!Files.exists(Path.of("build", "resources", "IOFile", fileName))) {
            jsonReader.apiNationalData(fileName);
        }
        fileName = "Statedata.txt";
        if (!Files.exists(Path.of("build", "resources", "IOFile", fileName))) {
            jsonReader.apiStateData(fileName);
        }

        this.listOfNationalData = listOfNationalFile();
        this.listOfStateData = listOfStateFile();


    }catch(IOException io){

          show = true;
        }
    }

    public List<NationalInformation> getListOfNationalData() {
        return listOfNationalData;
    }

    private List<NationalInformation> listOfNationalFile() throws IOException {

        listOfNationalData = new ArrayList<>();
        try (Stream<String> lines = Files.lines(Path.of("build", "resources", "IOFile", "Nationaldata.txt"))) {
            listOfNationalData = lines.map(line -> {
                        String[] data = line.split("\\W+");
                        return new NationalInformation(
                                data[0], data[1], data[2], data[3], data[4], data[5], data[6] + " " + data[7]
                        );
                    }
            ).collect(Collectors.toList());
        }
        return listOfNationalData;
    }


    private List<StateInformation> listOfStateFile() throws IOException {

        listOfStateData = new ArrayList<>();
        try (Stream<String> lines = Files.lines(Path.of("build", "resources", "IOFile", "Statedata.txt")).skip(1)) {
            listOfStateData = lines.map(line -> {
                        String[] data = line.split("\\W+");
                        return new StateInformation(
                                data[0], data[1], data[2], data[3], data[4], data[5], data[6], data[7], data[8]
                        );

                     }

            ).collect(Collectors.toList());
        }
        return listOfStateData;

    }

    public List<StateInformation> getListOfStateData() {
        return listOfStateData;
    }


    private Path getPath() throws IOException {
        OPPath = Path.of("build", "resources", "IOFile").toAbsolutePath();
        if (!Files.exists(OPPath)) {
            Files.createDirectories(OPPath);
        }
        return OPPath;
    }
}


StateInformation</br>

package fxcollection.models;

import java.util.Objects;

public class StateInformation {

    private Integer active;
    private Integer confirmed;
    private Integer deaths;
    private Integer deltaConfirmed;
    private Integer deltaDeaths;
    private Integer deltaRecovered;
    private Integer migratedOther;
    private Integer recovered;
    private String stateCode;


    public StateInformation(String active, String confirmed, String deaths, String deltaConfirmed, String deltaDeaths, String deltaRecovered, String migratedOther, String recovered, String stateCode) {
        this.active = Integer.valueOf(active);
        this.confirmed = Integer.valueOf(confirmed);
        this.deaths = Integer.valueOf(deaths);
        this.deltaConfirmed = Integer.valueOf(deltaConfirmed);
        this.deltaDeaths = Integer.valueOf(deltaDeaths);
        this.deltaRecovered = Integer.valueOf(deltaRecovered);
        this.migratedOther = Integer.valueOf(migratedOther);
        this.recovered = Integer.valueOf(recovered);
        this.stateCode = stateCode;

    }

    @Override
    public String toString() {
        return "Data of covid cases in a state{" +
                "recovered=" + recovered +
                ", active='" + active + '\'' +
                ", confirmed='" + confirmed + '\'' +
                ", deaths='" + deaths + '\'' +
                ", stateCode='" + stateCode + '\'' +
                '}';
    }



    public Integer getConfirmed() {
        return confirmed;
    }

    public String getStateCode() {
        return stateCode;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        StateInformation that = (StateInformation) o;
        return active.equals(that.active) &&
                confirmed.equals(that.confirmed) &&
                deaths.equals(that.deaths) &&
                deltaConfirmed.equals(that.deltaConfirmed) &&
                deltaDeaths.equals(that.deltaDeaths) &&
                deltaRecovered.equals(that.deltaRecovered) &&
                migratedOther.equals(that.migratedOther) &&
                recovered.equals(that.recovered) &&
                stateCode.equals(that.stateCode);
    }

    @Override
    public int hashCode() {
        return Objects.hash(active, confirmed, deaths, deltaConfirmed, deltaDeaths, deltaRecovered, migratedOther, recovered, stateCode);
    }
}


Package Views</br>

BarChartAverage</br>

package fxcollection.views;


import fxcollection.models.ReadAndStoreData;
import javafx.scene.chart.BarChart;
import javafx.scene.chart.CategoryAxis;
import javafx.scene.chart.NumberAxis;
import javafx.scene.chart.XYChart;

import java.util.Arrays;


public class BarChartAverage {

    private double[] average = new double[12];
    private ReadAndStoreData read;

    public ReadAndStoreData getRead() {
        return read;
    }

    public BarChartAverage() {
        this.read = new ReadAndStoreData();
    }

    public BarChartAverage(ReadAndStoreData read) {
        this.read = read;
    }
    enum Months{
        JANUARY,FEBRUARY,MARCH,APRIL,MAY,JUNE,JULY,AUGUST,SEPTEMBER,OCTOBER,NOVEMBER,DECEMBER

    }

    public BarChart<String, Number> barChartAverageOperations() {
        String[] monthsArray = otherOperations();
        for(int i=0;i< Months.values().length;i++ ) {
            final  int mvar  = i;
            average[i] = read.getListOfNationalData().stream().filter(date -> (date.getDate().split(" ")[1].equalsIgnoreCase(monthsArray[mvar]))).mapToInt(td -> Integer.parseInt(td.getDailyDeceased())).average().orElse(-1);
        }
        Double highestAvg= Arrays.stream(average).max().getAsDouble();
        BarChart<String, Number> bar = createBarChart(highestAvg);
        return insertBarChartData(monthsArray, average, bar);
    }

    private String[] otherOperations() {
        String months = Arrays.asList(Months.values()).toString();
        String monthsNew =  months.replaceAll("\\[","").replaceAll("\\]","");
        String withoutSpace = monthsNew.replaceAll("\\s+","").trim();
        String [] monthsArray =withoutSpace.split(",");
        return monthsArray;
    }


    private BarChart<String, Number> insertBarChartData(String[] monthsArray, double[] average, BarChart<String, Number> bar) {
        XYChart.Series<String,Number> series1 = new XYChart.Series<>();
        for(int i = 0; i< Months.values().length; i++) {

            series1.getData().add(new XYChart.Data<>(monthsArray[i], (average[i])));
        }
        bar.getData().add(series1);
        return bar;
    }


    private BarChart<String, Number> createBarChart(Double highestAvg) {
        CategoryAxis xaxis= new CategoryAxis();
        NumberAxis yaxis = new NumberAxis (0.0, highestAvg,20000);
        xaxis.setLabel("Months");
        yaxis.setLabel("Average death rate");
        BarChart<String,Number> bar = new BarChart<>(xaxis,yaxis);
        bar.setLegendVisible(false);
        return bar;
    }


}


BarChartDailyConfirmed</br>

package fxcollection.views;


import fxcollection.models.NationalInformation;
import fxcollection.models.ReadAndStoreData;
import javafx.scene.chart.BarChart;
import javafx.scene.chart.CategoryAxis;
import javafx.scene.chart.NumberAxis;
import javafx.scene.chart.XYChart;

import java.io.IOException;
import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;

public class BarChartDailyConfirmed {
    private ReadAndStoreData read;

    public BarChartDailyConfirmed() throws IOException {
        this.read = new ReadAndStoreData();
    }

    public BarChartDailyConfirmed(ReadAndStoreData read) {
        this.read = read;
    }

    public BarChart<String,Number> barChartDailyConfirmedOperations()  {

        List<NationalInformation> list = read.getListOfNationalData().stream().filter(date-> { return (date.getDate().split(" ")[0].equals("01"));
        }).collect(Collectors.toList());


        String highestDailyconfirmed = list.stream().max(Comparator.comparing(NationalInformation::getDailyConfirmed)).get().getDailyConfirmed();
        String leastDailyconfirmed = list.stream().min(Comparator.comparing(NationalInformation::getDailyConfirmed)).get().getDailyConfirmed();
        BarChart<String, Number> bar = createBarChart(highestDailyconfirmed, leastDailyconfirmed);
        insertDataInChart(list, bar);
        return bar;
    }

    private void insertDataInChart(List<NationalInformation> list, BarChart<String, Number> bar) {
        XYChart.Series<String,Number> series1 = new XYChart.Series<>();
        for(int i = 0; i< list.size(); i++) {

            series1.getData().add(new XYChart.Data<>(list.get(i).getDate().split(" ")[1], Integer.parseInt(list.get(i).getDailyConfirmed())));
        }
        bar.getData().add(series1);
    }


    private BarChart<String, Number> createBarChart(String highestDailyconfirmed, String leashDailyconfirmed) {
        CategoryAxis xaxis= new CategoryAxis();
        NumberAxis yaxis = new NumberAxis (Double.parseDouble(leashDailyconfirmed),Double.parseDouble(highestDailyconfirmed),20000);
        xaxis.setLabel("Months");
        yaxis.setLabel("daily confirmed cases");
        BarChart<String,Number> bar = new BarChart<>(xaxis,yaxis);
        bar.setLegendVisible(false);
        return bar;
    }
}



BarChartState</br>

package fxcollection.views;


import fxcollection.models.ReadAndStoreData;
import fxcollection.models.StateInformation;
import javafx.scene.chart.BarChart;
import javafx.scene.chart.CategoryAxis;
import javafx.scene.chart.NumberAxis;
import javafx.scene.chart.XYChart;

import java.util.Comparator;
import java.util.HashMap;
import java.util.Map;


public class BarChartState {
    private ReadAndStoreData read;


    public BarChartState() {
        this.read = new ReadAndStoreData();
    }

    public BarChartState(ReadAndStoreData read){
        this.read= read;

    }

    public  BarChart<String,Number> createBarChart() {

        String highestConfirmed = String.valueOf(read.getListOfStateData().stream().max(Comparator.comparing(StateInformation::getConfirmed)).get().getConfirmed());
        String leastConfirmed =  String.valueOf(read.getListOfStateData().stream().min(Comparator.comparing(StateInformation::getConfirmed)).get().getConfirmed());
        CategoryAxis xaxis= new CategoryAxis();
        NumberAxis yaxis = new NumberAxis (Double.parseDouble(leastConfirmed),Double.parseDouble(highestConfirmed),20000);
        xaxis.setLabel("States");
        yaxis.setLabel("Confirmed cases");
        return makeHashMap(xaxis, yaxis);
    }

    private BarChart<String, Number> makeHashMap(CategoryAxis xaxis, NumberAxis yaxis) {
        BarChart<String,Number> bar = new BarChart<>(xaxis, yaxis);
        XYChart.Series<String,Number> series1 = new XYChart.Series<>();
        Map<String, Number> stateMap = new HashMap<>();
        for(int i =0;i<read.getListOfStateData().size();i++)
            stateMap.put(read.getListOfStateData().get(i).getStateCode(),read.getListOfStateData().get(i).getConfirmed());
        return insertDataInBarChart(bar, series1, stateMap);
    }


    private BarChart<String, Number> insertDataInBarChart(BarChart<String, Number> bar, XYChart.Series<String, Number> series1, Map<String, Number> stateMap) {
        stateMap.forEach((k,v)->{
            XYChart.Data<String, Number> data = new XYChart.Data<>(k, v);
            series1.getData().add(data);

        });
        bar.setLegendVisible(false);
        bar.getData().add(series1);
        return bar;
    }

}



CovidInfoDisplays</br>

package fxcollection.views;

import fxcollection.models.Categorizing;
import fxcollection.models.CovidCasesNationDisplay;
import fxcollection.models.NationalInformation;
import fxcollection.models.ReadAndStoreData;
import javafx.collections.ObservableList;
import javafx.geometry.Insets;
import javafx.scene.control.ComboBox;
import javafx.scene.control.ListCell;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.HBox;
import javafx.scene.paint.Color;
import javafx.scene.text.Font;
import javafx.scene.text.FontPosture;
import javafx.scene.text.FontWeight;
import javafx.scene.text.Text;
import javafx.util.StringConverter;

import java.io.IOException;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

import static javafx.collections.FXCollections.observableArrayList;

public class CovidInfoDisplays {

    private ComboBox<CovidCasesNationDisplay> priority;
    private ComboBox<NationalInformation> value;
    private final Text detailsTextBox;
    private final Text guideLines;
    private final Map<CovidCasesNationDisplay, List<NationalInformation>> caswmap;
    private final ObservableList<CovidCasesNationDisplay> dataOf;
    private Categorizing category;
    private HBox hBox;



    public CovidInfoDisplays() {
        category = new Categorizing();
        caswmap =  category.getMapData();
        dataOf = observableArrayList(caswmap.keySet());
        detailsTextBox = new Text();
        guideLines = new Text();
        setUpPriority();
        setUpValue();
    }

    public CovidInfoDisplays(ReadAndStoreData read){
        category = new Categorizing(read);
        caswmap =  category.getMapData();
        dataOf = observableArrayList(caswmap.keySet());
        detailsTextBox = new Text();
        guideLines = new Text();
        this.hBox = new HBox();
        setUpPriority();
        setUpValue();
    }



    private class CovidStringConvertor extends StringConverter<NationalInformation> {
        private final String SEP = ", date ";
        @Override
        public String toString(NationalInformation caseTimeSeriesModel) {
            if (caseTimeSeriesModel == null)
                return null;

            else
                return "confirmed cases:" + " " + caseTimeSeriesModel.getDailyConfirmed()+ SEP + caseTimeSeriesModel.getDate();
        }


        @Override
        public NationalInformation fromString(String string) {
            String date = string.split(SEP)[1];
            List<NationalInformation> casesList = category.getCovidInfo().stream()
                    .filter(model -> model.getDate().equals(date))
                    .limit(1)
                    .collect(Collectors.toList());
            return casesList.isEmpty() ? null : casesList.get(0);

        }
    }

    private ObservableList<CovidCasesNationDisplay> sortCases() {
            return dataOf.sorted((o1, o2) -> {
                if (o1.getMax() < o2.getMax())
                    return -1;

                if (o1.getMax() > o2.getMax())
                    return 1;
                else
                    return 0;
            });
        }

        private void setUpPriority() {
            priority = new ComboBox<>();
            priority.getItems().addAll(sortCases());
            priority.setPromptText("--Select a Month--");
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
     createDataSelectionListenerForGuideLines();
     value.setVisible(false);
     handleValueComboBoxUpdate();
    }


    private void handleValueComboBoxUpdate(){
        value.setButtonCell(new ListCell<>(){
            @Override
            protected void updateItem(NationalInformation caseTimeSeriesModel, boolean empty){
                super.updateItem(caseTimeSeriesModel,empty);
                        if(empty || caseTimeSeriesModel == null)
                            setText("click here");
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
                +"confirmed:"+newValue.getDailyConfirmed()+"\n"
                        +"deceased:"+newValue.getDailyDeceased()+"\n"
                        +"recovered:"+newValue.getDailyRecovered()+"\n";
                detailsTextBox.setText(displayText);
                detailsTextBox.setVisible(true);
            }
        });

    }

    private void createDataSelectionListenerForGuideLines(){

        String display=" How to protect yourself from corona virus:" + "\n"
                        +"-Wash your hands often" + "\n"
                        +"-Wear mask when you are outside"+"\n"
                        +"-Limit social gatherings and avoid crowded places"+"\n"
                        +"-Folllow social distancing"+"\n"
                        +"-Avoid close contact with someone who is sick"+"\n"
                        +"-Clean and disinfect objects and surfaces that are touched frequently"+"\n";

                guideLines.setText(display);
                guideLines.setVisible(true);
                guideLines.setFill(Color.GREEN);
                guideLines.setFont(Font.font("Arial", FontWeight.BOLD, FontPosture.REGULAR, 12));

    }

    private void setUpHBox(){
        hBox.setSpacing(10);
        ComboBox<CovidCasesNationDisplay> covFirst = getPriority();
        ComboBox<NationalInformation> covSecond = getValue();
        Text detailsTextBox = getDetailsTextBox();
        hBox.getChildren().addAll(covFirst,covSecond,detailsTextBox);
        hBox.setMargin(covFirst,new Insets(20,5,5,10));
        hBox.setMargin(covSecond,new Insets(20, 5, 5,5));
        hBox.setMargin(detailsTextBox,new Insets( 20,5,5,5));
    }

    public void setUpBorderPane(BorderPane pane){

        hBox= new HBox();
        setUpHBox();
        pane.setTop(hBox);
        Text guideLines = getGuideLines();
        pane.setBottom(guideLines);
        pane.setMargin(guideLines,new Insets( 20,5,5,10));
    }


    public ComboBox<CovidCasesNationDisplay> getPriority() {
        return priority;
    }

    public ComboBox<NationalInformation> getValue() {
        return value;
    }

    public Text getDetailsTextBox() {
        return detailsTextBox;
    }

    public Text getGuideLines() {
        return guideLines;
    }


}



PieChartNational</br>

package fxcollection.views;


import fxcollection.models.ReadAndStoreData;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.scene.chart.PieChart;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class PieChartNational {

    private ReadAndStoreData read;


    public PieChartNational() {
        this.read = new ReadAndStoreData();
    }

    public PieChartNational(ReadAndStoreData read)  {
        this.read = read;
    }

    public PieChart pieChart()  {
        return pieChartOperations();
    }

    private PieChart pieChartOperations()  {
        Integer size =   read.getListOfNationalData().size();

        String totalConfirmed =read.getListOfNationalData().get(size-1).getTotalConfirmed();
        String totalRecovered =read.getListOfNationalData().get(size-1).getTotalRecovered();
        String totalDeceased =read.getListOfNationalData().get(size-1).getTotalDeceased();

        int sum = Integer.parseInt(totalConfirmed)+Integer.parseInt(totalRecovered)+Integer.parseInt(totalDeceased);
        String totRecovered =String.valueOf (Math.round((Double.parseDouble(totalRecovered)/sum)*100));
        String totDeceased =String.valueOf(Math.round((Double.parseDouble(totalDeceased)/sum)*100));
        String totConfirmed = String.valueOf(Math.round((Double.parseDouble(totalConfirmed)/sum)*100));

        Map<String, Double> pieMap = pieChartOperations(totalConfirmed,totConfirmed, totalRecovered,totRecovered, totalDeceased,totDeceased);
        return insertDataInPieChart(pieMap);
    }


    private PieChart insertDataInPieChart(Map<String, Double> pieMap) {
        List<PieChart.Data> pd = new ArrayList<>();
        pieMap.forEach((key, value)->pd.add(new PieChart.Data(key,value)));
        ObservableList<PieChart.Data> pieChart = FXCollections.observableArrayList(pd);
        final PieChart chart = new PieChart(pieChart);
        return chart;
    }


    private Map<String, Double> pieChartOperations(String totalConfirmed,String totConfirmed, String totalRecovered,String totRecovered, String totalDeceased,String totDeceased) {
        Map<String, Double> pieMap = new HashMap<>();

        pieMap.put("Total confirmed"+" "+totConfirmed+".0%",Double.parseDouble(totalConfirmed));
        pieMap.put("Total recovered"+" "+totRecovered+".0%",Double.parseDouble(totalRecovered));
        pieMap.put("Total deceased"+" "+totDeceased+".0%",Double.parseDouble(totalDeceased));
        return pieMap;
    }
}




RadioButtonBack</br>


package fxcollection.views;


import fxcollection.models.ReadAndStoreData;
import javafx.beans.value.ChangeListener;
import javafx.beans.value.ObservableValue;
import javafx.scene.control.RadioButton;
import javafx.scene.control.Toggle;
import javafx.scene.control.ToggleGroup;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.HBox;
import javafx.scene.layout.VBox;

import java.io.IOException;

public class RadiobuttonBack {

    private RadioButton rb1, rb2, rb3,rb4;
    private BorderPane pane1;
    private VBox vBox;
    private final ToggleGroup group;
    private BarChartAverage bChart;
    private BarChartDailyConfirmed dailyConfirmed;
    private PieChartNational pieChart;
    private BarChartState barChartState;
    private ReadAndStoreData read;
    private HBox hBox;

    public RadiobuttonBack(VBox vBox,BorderPane pane1,ReadAndStoreData read) throws IOException {
        this.read = read;
        this.bChart = new BarChartAverage(read);
        this.dailyConfirmed = new BarChartDailyConfirmed(read);
        this.pieChart = new PieChartNational(read);
        this.barChartState = new BarChartState(read);
        this.vBox = vBox;
        this.pane1=pane1;
        this.group = new ToggleGroup();
        this.hBox = new HBox();

    }

    private RadioButton createRadioButton(String label, String userData) {
        RadioButton rb = new RadioButton(label);
        rb.setToggleGroup(group);
        rb.setUserData(userData);
        return rb;
    }

    public void radioButtonForCharts() {

        rb1 = createRadioButton("Total confirmed,Total recovered,Total deceased covid-19 cases", "piechart");
        rb2 = createRadioButton("Death rate per month in the year 2020","Barchartwo");
        rb3 = createRadioButton("Tracking daily confirmed cases with an interval of 30 days ","Barchartone");
        rb4 = createRadioButton("Confirmed covid cases of each state","barcharthree");
        hBox.getChildren().add(vBox);
        vBox.getChildren().addAll(rb1, rb2, rb3,rb4);
        radioButtonOperations();
    }

    private void radioButtonOperations() {
        group.selectedToggleProperty().

                addListener(new ChangeListener<Toggle>() {
                    public void changed(ObservableValue<? extends Toggle> ov,
                                        Toggle old_toggle, Toggle new_toggle) {
                        if (group.getSelectedToggle() != null) {
                            System.out.println(group.getSelectedToggle().getUserData().toString());
                            if (group.getSelectedToggle().getUserData() == "piechart") {
                                pane1.setCenter(pieChart.pieChart());
                            } else if (group.getSelectedToggle().getUserData() == "Barchartwo") {

                                pane1.setCenter(bChart.barChartAverageOperations());

                            } else if (group.getSelectedToggle().getUserData() == "Barchartone") {
                                pane1.setCenter(dailyConfirmed.barChartDailyConfirmedOperations());

                            } else if (group.getSelectedToggle().getUserData() == "barcharthree") {
                                pane1.setCenter(barChartState.createBarChart());

                            }
                        }
                    }
                });
    }

}


CovidApp</br>

import fxcollection.models.ReadAndStoreData;
import fxcollection.views.CovidInfoDisplays;
import fxcollection.views.RadiobuttonBack;
import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.HBox;
import javafx.scene.layout.VBox;
import javafx.scene.paint.Color;
import javafx.scene.text.Font;
import javafx.scene.text.FontWeight;
import javafx.scene.text.Text;
import javafx.stage.Stage;

import java.io.IOException;



public class CovidApp extends Application {

        private CovidInfoDisplays comboBoxes;
        private RadiobuttonBack radiobutton;
        private ReadAndStoreData read;

        public static void main(String[] args)  {
            launch(args);
        }

       @Override
       public void start(Stage primaryStage) throws IOException {
            read = new ReadAndStoreData();
            if(read.isShow()){
                thirdScene();
           }
           else {
               comboBoxes = new CovidInfoDisplays(read);
               BorderPane borderPane = new BorderPane();
               comboBoxes.setUpBorderPane(borderPane);
               Scene sceneOne = new Scene(borderPane, 900, 230);
               primaryStage.setScene(sceneOne);
               primaryStage.setTitle("Covid 19");
               primaryStage.show();
               secondScene();

           }
        }

        private void secondScene() throws IOException {
        BorderPane pane1 = new BorderPane();
        Scene scene = new Scene(pane1, 900, 600);
        Stage stage1 = new Stage();
        HBox hBox = new HBox();
        VBox vbox = new VBox();

        radiobutton = new RadiobuttonBack(vbox,pane1,read);
        radiobutton.radioButtonForCharts();

        hBox.getChildren().add(vbox);
        hBox.setSpacing(50);
        vbox.setSpacing(10);
        hBox.setPadding(new Insets(20, 10, 10, 20));
        pane1.setTop(hBox);
        stage1.setScene(scene);
        stage1.setTitle("Covid-19 cases graphs");
        stage1.show();

        }

        private void thirdScene(){
            Button button = new Button("Ok");
            BorderPane pane = new BorderPane();
            Text text = new Text();
            Scene scene = new Scene(pane, 300, 90);
            final Stage stage2 = new Stage();
            VBox vbox = new VBox();
            HBox hbox = new HBox();
            text.setText("Sorry, unable to retrieve data.");
            text.setFont(Font.font("Verdana", FontWeight.BOLD, 13));
            text.setFill(Color.RED);
            setUpHbox(button, pane, text, scene, stage2, vbox, hbox);
            button.setOnAction(actionEvent ->  {
                stage2.close();
            });
        }

    private void setUpHbox(Button button, BorderPane pane, Text text, Scene scene, Stage stage2, VBox vbox, HBox hbox) {
        hbox.getChildren().add(vbox);
        hbox.getChildren().add(text);
        pane.setCenter(button);
        hbox.setPadding(new Insets(10, 7, 7, 7));
        pane.setTop(hbox);
        stage2.setScene(scene);
        stage2.show();
        stage2.setTitle("Error Message");
    }
}













