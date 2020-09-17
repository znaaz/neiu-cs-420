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

IOFile
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



import java.io.File;


public interface IOFileImp {
         File openFile(String fileName);
         String readTextFromFile(File file);
         void writeTextOnFile(String content,String file);
    }</br>



JSONReader
package API;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.URL;
import java.net.URLConnection;

public class JSONReader implements JSONReaderImp {

    @Override
    public void parseJSON(String data, String fileName)
        {
            String jsonData = "";
        try{
            JSONParser parser = new JSONParser();
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



JSONReaderImp
package API;

public interface JSONReaderImp {

    void parseJSON(String data, String fileName);
    String readDataFromURL(String api);
}</br>




Main
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











