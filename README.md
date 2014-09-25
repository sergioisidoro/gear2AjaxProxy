
On the Function onRecieve, create a HTTP request sent to the URL, get the response, and send back to device


	public void onReceive(int channelId, byte[] data) {
			Log.d(TAG, "onReceive");
			String ajaxRequest = new String(data);
			
			HttpClient httpclient = new DefaultHttpClient();
		   
			try {
				JSONObject jObject = new JSONObject(ajaxRequest);
				String reqUrl = jObject.getString("url");	
				HttpPost httppost = new HttpPost(reqUrl);
				
				// ADD SPECIFIC JSON TAGS HERE !!!
				
				//List<NameValuePair> nameValuePairs = new ArrayList<NameValuePair>(2);
		        //nameValuePairs.add(new BasicNameValuePair("id", "12345"));
		        //nameValuePairs.add(new BasicNameValuePair("stringdata", "AndDev is Cool!"));
		        //httppost.setEntity(new UrlEncodedFormEntity(nameValuePairs));

		        // Execute HTTP Post Request
		        HttpResponse response = httpclient.execute(httppost);
		        final String rawHttp = response.toString();
		        
		        final HelloAccessoryProviderConnection uHandler = mConnectionsMap.get(Integer
						.parseInt(String.valueOf(mConnectionId)));
				if(uHandler == null){
					Log.e(TAG,"Error, can not get HelloAccessoryProviderConnection handler");
					return;
				}
				new Thread(new Runnable() {
					public void run() {
						try {
							uHandler.send(HELLOACCESSORY_CHANNEL_ID, rawHttp.getBytes());
						}
						catch (IOException e) {
					        // TODO Auto-generated catch block
					    } 
					}
				}).start();
		              
			} catch (JSONException e1) {
				// TODO Auto-generated catch block
				e1.printStackTrace();
			}
			  catch (ClientProtocolException e) {
		        // TODO Auto-generated catch block
		    } catch (IOException e) {
		        // TODO Auto-generated catch block
		    } 
			
		}
