package com.tcs.bancs.microservices.controller;

import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.ArrayList;
import java.util.Base64;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;
import java.util.Properties;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.commons.lang3.StringUtils;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.google.gson.Gson;
import com.tcs.bancs.microservices.StausCountResBean;
import com.tcs.bancs.microservices.aggregator.AggregationServiceImpl;
import com.tcs.bancs.microservices.config.CacheConfig;
import com.tcs.bancs.microservices.configuration.PropertyLoader;
import com.tcs.bancs.microservices.db.model.ADLH;
 
import com.tcs.bancs.microservices.repository.day.Adesh_dec_encRepo;
import com.tcs.bancs.microservices.repository.day.Login_flag_checkRepo;
import com.tcs.bancs.microservices.services.logtableupdatet1t2t3;
//import com.tcs.bancs.microservices.services.logtableupdatet1t2t3;
import com.tcs.bancs.microservices.util.DBProcess;
import com.tcs.bancs.microservices.util.FrameworkConstants;


@RestController
 
@RequestMapping("/Dateapi")
public class FileStatusCountController {
	Logger logger = LoggerFactory.getLogger(FileStatusCountController.class);
	String ErrorCodeMasterFilePath = CacheConfig.frameworkConfigProperties.getProperty(FrameworkConstants.LOOKUP_FILES_PATH);
	Properties error = PropertyLoader.readPropertyFile(new String(ErrorCodeMasterFilePath + "/ErrorCodeMaster.properties"));
	Properties parameter=PropertyLoader.readPropertyFile(new String(ErrorCodeMasterFilePath + "/ServiceParameters.properties"));
	@Autowired
	Adesh_dec_encRepo Adesh_dec_encRepo;
	
	@Autowired
	DBProcess dbprocess;
	
	@Autowired
	Login_flag_checkRepo lf;
	
	
//	@Autowired
//	logtableupdatet1t2t3 update;
	
//	@Autowired
//	AdlhDetailDayRepo AdlhDetailDayRepo;
	
	
	@Autowired
	AggregationServiceImpl aggregationServiceImpl;
	 
	@Autowired
	logtableupdatet1t2t3 update;
	 
	@SuppressWarnings("unchecked")
	
	@PostMapping(value="/Date" ,produces = { "application/json" } )
	@CrossOrigin(origins="http://localhost:4200")	
	public ResponseEntity<String> getChannelEnqById(@RequestParam("data") String jsonData, HttpServletRequest request,HttpServletResponse response)
			throws Exception
		{
		 
		 
			String filetype="";
			 String txn_no = "111001";
			String  txn_name = "CHART";
		 
			//HashMap<String, String> lastlogger = new HashMap<String, String>();
			  
//				MandatesUploadResBean resp = new MandatesUploadResBean();
//				List<MandatesUploadResBean.FileStatus> fileStatus=new ArrayList<>();
	 
		Gson gson = new Gson();
		StausCountResBean resp = new StausCountResBean();
		String finalResponse = new String();		
		String CLIENT_IP = request.getLocalAddr();	
		ObjectMapper objmap = new ObjectMapper();
		Map<String, Object> data = new HashMap<>();
		data = objmap.readValue(jsonData, Map.class);
		
		String selected_date = (String) data.get("SELECTED_DATE");
		String reference_number = (String) data.get("REQUEST_REFERENCE_NUMBER");
		String source_id = (String) data.get("SOURCE_ID");
		String teller_id = (String) data.get("REQUEST_TELLER_ID");
		System.out.println(teller_id);
		DateTimeFormatter Date = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss:SSS");
		LocalDateTime time = LocalDateTime.now();
		String RECD_TIME=Date.format(time);
		
		if(reference_number==null && source_id==null && teller_id==null && selected_date==null){
			
			logger.error("Request Data is empty");			
			resp.setREQUEST_REFERENCE_NUMBER(null);
			resp.setSOURCE_ID(null);
			resp.setRESPONSE_STATUS(null);
			resp.setERROR_CODE("ER016");
			System.out.println(error.getProperty("ER016"));
			resp.setERROR_DESCRIPTION(error.getProperty("ER016"));
			resp.setSELECTED_DATE(null);
			finalResponse=gson.toJson(resp);
//			boolean status=update.updatetable(reference_number,source_id,CLIENT_IP,teller_id,"1","ER016", error.getProperty("ER016"),Base64.getEncoder().encodeToString((jsonData).getBytes()),"Failure",RECD_TIME,Base64.getEncoder().encodeToString((finalResponse).getBytes()));
//			if(status){
//				logger.info("Database is updated: ALL NULL");
//			}
//			else{
//				logger.error("Error during the Database updation: ALL NULL");
//			}
			
			return new ResponseEntity<String>(finalResponse, HttpStatus.OK);
			
		}
		else {
			if(reference_number==null || reference_number.equals(" ") || reference_number=="" ) {
				
				logger.error("Request Reference number is empty");
				resp.setREQUEST_REFERENCE_NUMBER(reference_number);
				resp.setSOURCE_ID(source_id);
				resp.setRESPONSE_STATUS("1");
				resp.setERROR_CODE("ER015");
				resp.setERROR_DESCRIPTION(error.getProperty("ER015"));
				resp.setSELECTED_DATE(selected_date);
				finalResponse=gson.toJson(resp);
//				boolean status=update.updatetable(reference_number,source_id,CLIENT_IP,teller_id,"1","ER015", error.getProperty("ER015"),Base64.getEncoder().encodeToString((jsonData).getBytes()),"Failure",RECD_TIME,Base64.getEncoder().encodeToString((finalResponse).getBytes()));
//				if(status){
//					logger.info("Database is updated: REF number null");
//				}
//				else{
//					logger.error("Error during the Database updation: REF number null");
//				}
				
				return new ResponseEntity<String>(finalResponse, HttpStatus.OK);
				
			}else if(reference_number.length()!=25) {
			
			logger.error("1:Request Reference number is invalid");
			resp.setREQUEST_REFERENCE_NUMBER(reference_number);
			resp.setSOURCE_ID(source_id);
			resp.setRESPONSE_STATUS("1");
			resp.setERROR_CODE("ER015");
			resp.setERROR_DESCRIPTION(error.getProperty("ER015"));
			resp.setSELECTED_DATE(selected_date);
			finalResponse=gson.toJson(resp);
//			boolean status=update.updatetable(reference_number,source_id,CLIENT_IP,teller_id,"1","ER015", error.getProperty("ER015"),Base64.getEncoder().encodeToString((jsonData).getBytes()),"Failure",RECD_TIME,Base64.getEncoder().encodeToString((finalResponse).getBytes()));
//			if(status){
//				logger.info("Database is updated: REF number length NE");
//			}
//			else{
//				logger.error("Error during the Database updation: REF number length NE");
//			}
			
			return new ResponseEntity<String>(finalResponse, HttpStatus.OK);
			
		}else if(reference_number.length()==25){
			// execute till here 
			String sbi = reference_number.substring(0, 3);
			String s = reference_number.substring(3, reference_number.length());
			if(sbi.equals("SBI") &&  reference_number.matches("^(?![0-9]*$)[a-zA-Z0-9]+$") && StringUtils.isAlphanumeric(s)) 
			{	
				// logic ??  
				
//				logger.error("2:Request Reference number == 25 else if");
			}
			else {
				
				logger.error("2:Request Reference number is invalid");
				resp.setREQUEST_REFERENCE_NUMBER(reference_number);
				resp.setSOURCE_ID(source_id);
				resp.setRESPONSE_STATUS("1");
				resp.setERROR_CODE("ER015");
				resp.setERROR_DESCRIPTION(error.getProperty("ER015"));
				resp.setSELECTED_DATE(selected_date);
				finalResponse = gson.toJson(resp);
//				boolean status=update.updatetable(reference_number,source_id,CLIENT_IP,teller_id,"1","ER015", error.getProperty("ER015"),Base64.getEncoder().encodeToString((jsonData).getBytes()),"Failure",RECD_TIME,Base64.getEncoder().encodeToString((finalResponse).getBytes()));
//				if(status){
//					logger.info("Database is updated: == 25");
//				}
//				else{
//					logger.error("Error during the Database updation: == 25");
//				}
				
				return new ResponseEntity<String>(finalResponse, HttpStatus.OK);
				
			}
		}
		if(teller_id==null || teller_id.trim().isEmpty()) {
			// logic ??
			logger.error("teller_id is null");
		}
//		else if(teller_id.equals(" ") || teller_id=="" ) {
//			
//			logger.error("Request teller Id is empty");
//			resp.setREQUEST_REFERENCE_NUMBER(reference_number);
//			resp.setSOURCE_ID(source_id);
//			resp.setRESPONSE_STATUS("1");
//			resp.setERROR_CODE("ER023");
//			resp.setERROR_DESCRIPTION(error.getProperty("ER023"));
//			resp.setSELECTED_DATE(selected_date);
//			finalResponse=gson.toJson(resp);
////			boolean status=update.updatetable(reference_number,source_id,CLIENT_IP,teller_id,"1","ER023", error.getProperty("ER023"),Base64.getEncoder().encodeToString((jsonData).getBytes()),"Failure",RECD_TIME,Base64.getEncoder().encodeToString((finalResponse).getBytes()));
////			if(status){
////				logger.info("Database is updated: NULL teller ID");
////			}
////			else{
////				logger.error("Error during the Database updation: NULL teller ID");
////			}
//			
//			return new ResponseEntity<String>(finalResponse, HttpStatus.OK);
//			
//		}
		else if(!StringUtils.isNumeric(teller_id)) {
			
			logger.error("1:Request teller Id is invalid");
			resp.setREQUEST_REFERENCE_NUMBER(reference_number);
			resp.setSOURCE_ID(source_id);
			resp.setRESPONSE_STATUS("1");
			resp.setERROR_CODE("ER023");
			resp.setERROR_DESCRIPTION(error.getProperty("ER023"));
			resp.setSELECTED_DATE(selected_date);
			finalResponse=gson.toJson(resp);
//			boolean status=update.updatetable(reference_number,source_id,CLIENT_IP,teller_id,"1","ER023", error.getProperty("ER023"),Base64.getEncoder().encodeToString((jsonData).getBytes()),"Failure",RECD_TIME,Base64.getEncoder().encodeToString((finalResponse).getBytes()));
//			if(status){
//				logger.info("Database is updated");
//			}
//			else{
//				logger.error("Error during the Database updation");	
//			}
			
			return new ResponseEntity<String>(finalResponse, HttpStatus.OK);
			
		}else {
			
//		teller_id=StringUtils.leftPad(teller_id, 16, "0");	
//		
//		// telm to adl
////		List<Telm> telm = new ArrayList<Telm>();
//		ArrayList<ADLH> Adlh = new ArrayList<ADLH>();
//		
//		ArrayList<Object> queryParams1 = new ArrayList<>();
////		queryParams1.add("003");
//		queryParams1.add(teller_id);
//		
//		// Check AdlhDetail
//		Adlh = dbprocess.fetchRepositories(null, "Adlh", "AdlhDetail", "findByAdlhDetails", false, true,queryParams1);
////		System.out.println(Adlh);
//		if(Adlh.isEmpty()) {
//			logger.error("2:Request teller Id is invalid");
//			resp.setREQUEST_REFERENCE_NUMBER(reference_number);
//			resp.setSOURCE_ID(source_id);
//			resp.setRESPONSE_STATUS("1");
//			resp.setERROR_CODE("ER023");
//			resp.setERROR_DESCRIPTION(error.getProperty("ER023"));
//			resp.setSELECTED_DATE(selected_date);
//			finalResponse=gson.toJson(resp);
////			boolean status=update.updatetable(reference_number,source_id,CLIENT_IP,teller_id,"1","ER023", error.getProperty("ER023"),Base64.getEncoder().encodeToString((jsonData).getBytes()),"Failure",RECD_TIME,Base64.getEncoder().encodeToString((finalResponse).getBytes()));
////			if(status){
////				logger.info("Database is updated: Invalid teller ID");
////			}
////			else{
////				logger.error("Error during the Database updation: Invalid teller ID");
////			}
//			
//			return new ResponseEntity<String>(finalResponse, HttpStatus.OK);
//			
//		}else {
//			
////			String sign_on_flag = telm.get(0).getSIGNON_FLAG();
//			String sign_on_flag = Adlh.get(0).getLoginflag();
//			
//			logger.info(sign_on_flag);
//			
//			if (!sign_on_flag.equalsIgnoreCase("Y")) {
//				logger.error("User not Signed On");
//				resp.setREQUEST_REFERENCE_NUMBER(reference_number);
//				resp.setSOURCE_ID(source_id);
//				resp.setRESPONSE_STATUS("1");
//				resp.setERROR_CODE("ER024");
//				resp.setERROR_DESCRIPTION(error.getProperty("ER024"));
//				resp.setSELECTED_DATE(selected_date);
//				finalResponse=gson.toJson(resp);
////				boolean status=update.updatetable(reference_number,source_id,CLIENT_IP,teller_id,"1","ER024", error.getProperty("ER024"),Base64.getEncoder().encodeToString((jsonData).getBytes()),"Failure",RECD_TIME,Base64.getEncoder().encodeToString((finalResponse).getBytes()));
////				if(status){
////					logger.info("Database is updated: signon FLAG YYYYY");
////				}
////				else{
////					logger.error("Error during the Database updation: signon FLAG YYYYY");
////				}
//				
//				return new ResponseEntity<String>(finalResponse, HttpStatus.OK);
//				
//				}	
//			}
			
			teller_id=StringUtils.leftPad(teller_id, 7, "0");
			int count_of_teller_active=0;
			try{
						count_of_teller_active=lf.findCountByAdlhDetails(teller_id);
			}catch(Exception e){
				 logger.error("Database access error while fectching count for teller number");
			}
			if (count_of_teller_active<1) {
				logger.error("User not Signed On");
				resp.setREQUEST_REFERENCE_NUMBER(reference_number);
				resp.setSOURCE_ID(source_id);
				resp.setRESPONSE_STATUS("1");
				resp.setERROR_CODE("ER024");
				resp.setERROR_DESCRIPTION(error.getProperty("ER024"));
				finalResponse=gson.toJson(resp);
//				boolean status=update.updatetable(reference_number,source_id,CLIENT_IP,teller_id,"1","ER024", error.getProperty("ER024"),Base64.getEncoder().encodeToString((jsonData).getBytes()),"Failure",RECD_TIME,Base64.getEncoder().encodeToString((finalResponse).getBytes()),txn_no,txn_name);
//				if(status){
//					logger.info("Database is updated");
//				}
//				else{
//					logger.error("Error during the Database updation");
//				}
				
				return new ResponseEntity<String>(finalResponse, HttpStatus.BAD_REQUEST);
			
			} 
			
			
			
		}
		if(source_id==null || source_id.equals(" ") || source_id=="") {
			
			logger.error("Source Id is empty");
			resp.setREQUEST_REFERENCE_NUMBER(reference_number);
			resp.setSOURCE_ID(source_id);
			resp.setRESPONSE_STATUS("1");
			resp.setERROR_CODE("ER020");
			resp.setERROR_DESCRIPTION(error.getProperty("ER020"));
			resp.setSELECTED_DATE(selected_date);
			finalResponse=gson.toJson(resp);
//			boolean status=update.updatetable(reference_number,source_id,CLIENT_IP,teller_id,"1","ER020", error.getProperty("ER020"),Base64.getEncoder().encodeToString((jsonData).getBytes()),"Failure",RECD_TIME,Base64.getEncoder().encodeToString((finalResponse).getBytes()));
//			if(status){
//				logger.info("Database is updated");
//			}
//			else{
//				logger.error("Error during the Database updation");
//			}
//			
			return new ResponseEntity<String>(finalResponse, HttpStatus.OK);
			
		}else if(source_id.length() > 5 || !StringUtils.isAlphanumeric(source_id)) {
			
			logger.error("Source Id is invalid");
			resp.setREQUEST_REFERENCE_NUMBER(reference_number);
			resp.setSOURCE_ID(source_id);
			resp.setRESPONSE_STATUS("1");
			resp.setERROR_CODE("ER020");
			resp.setERROR_DESCRIPTION(error.getProperty("ER020"));
			resp.setSELECTED_DATE(selected_date);
			finalResponse=gson.toJson(resp);
//			boolean status=update.updatetable(reference_number,source_id,CLIENT_IP,teller_id,"1","ER020", error.getProperty("ER020"),Base64.getEncoder().encodeToString((jsonData).getBytes()),"Failure",RECD_TIME,Base64.getEncoder().encodeToString((finalResponse).getBytes()));
//			if(status){
//				logger.info("Database is updated");
//			}
//			else{
//				logger.error("Error during the Database updation");
//			}
			
			return new ResponseEntity<String>(finalResponse, HttpStatus.OK);
			
		}
		if(selected_date==null || selected_date.equals(" ") || selected_date=="") {
			logger.error("Selected Date is empty");
			resp.setREQUEST_REFERENCE_NUMBER(reference_number);
			resp.setSOURCE_ID(source_id);
			resp.setRESPONSE_STATUS("1");
			resp.setERROR_CODE("ER038");
			resp.setERROR_DESCRIPTION(error.getProperty("ER038"));
			resp.setSELECTED_DATE(selected_date);
			finalResponse=gson.toJson(resp);
//			boolean status=update.updatetable(reference_number,source_id,CLIENT_IP,teller_id,"1","ER038", error.getProperty("ER038"),Base64.getEncoder().encodeToString((jsonData).getBytes()),"Failure",RECD_TIME,Base64.getEncoder().encodeToString((finalResponse).getBytes()));
//			if(status){
//				logger.info("Database is updated");
//			}
//			else{
//				logger.error("Error during the Database updation");
//			}
			
			return new ResponseEntity<String>(finalResponse, HttpStatus.OK);
		}else {
			LocalDate given_date = LocalDate.parse(selected_date, DateTimeFormatter.ISO_LOCAL_DATE);
			LocalDate today = LocalDate.now();
			if(given_date.isBefore(today) || given_date.isEqual(today)) {
				
			}
			else {
				logger.error("Selected Date is invalid");
				resp.setREQUEST_REFERENCE_NUMBER(reference_number);
				resp.setSOURCE_ID(source_id);
				resp.setRESPONSE_STATUS("1");
				resp.setERROR_CODE("ER038");
				resp.setERROR_DESCRIPTION(error.getProperty("ER038"));
				resp.setSELECTED_DATE(selected_date);
				finalResponse=gson.toJson(resp);
//				boolean status=update.updatetable(reference_number,source_id,CLIENT_IP,teller_id,"1","ER038",error.getProperty("ER038"),Base64.getEncoder().encodeToString((jsonData).getBytes()),"Failure",RECD_TIME,Base64.getEncoder().encodeToString((finalResponse).getBytes()));
//				if(status){
//					logger.info("Database is updated");
//				}
//				else{
//					logger.error("Error during the Database updation");
//				}
				
				return new ResponseEntity<String>(finalResponse, HttpStatus.OK);
			}
		}
		List<Object[]> results=Adesh_dec_encRepo.findByDateDetails(selected_date);
		int[] status_count=new int[13];
		

		for(Object[] row:results) {
			Integer status=Integer.parseInt(row[0].toString());
			status_count[status]=Integer.parseInt(row[1].toString());

			
		}
	   resp.setREQUEST_REFERENCE_NUMBER(reference_number);
		resp.setSOURCE_ID(source_id);
		resp.setRESPONSE_STATUS("0");
		resp.setSELECTED_DATE(selected_date);
		resp.setSTATUS_COUNTS(status_count);
		finalResponse=gson.toJson(resp);
//		boolean status=update.updatetable(reference_number,source_id,CLIENT_IP,teller_id,"1","ER038", error.getProperty("ER038"),Base64.getEncoder().encodeToString((jsonData).getBytes()),"Failure",RECD_TIME,Base64.getEncoder().encodeToString((finalResponse).getBytes()));
//		if(status){
//			logger.info("Database is updated");
//		}
//		else{
//			logger.error("Error during the Database updation");
//		}
		
	   return new ResponseEntity<String>(finalResponse, HttpStatus.OK);
		}

}
}
