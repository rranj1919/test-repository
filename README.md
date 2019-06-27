public class OpportunityHistoryTrack {
    //private final ApexPages.StandardController controller;
    public OpportunityHistoryTrack(ApexPages.StandardController controller) {
		//this.controller = controller;
	}
    public void doInit(){
        String userID = userinfo.getuserid();
        String Query = 'SELECT Count() FROM History_Tracking__c WHERE Createddate = Today and OwnerID =: userID and EventAction__c = \'Visited\' and Object__c = \'Opportunity\'';
        System.debug('Query => ' + Query);
        Integer ct = Database.countQuery(Query);
        if (ct == 0){
            //String dt = String.valueOf(Date.today());
            //dt = userID + '-' + dt; 
            History_Tracking__c hist = new History_Tracking__c (Counter__c = 1,EventAction__c = 'Visited',Object__c='Opportunity');
            //History_Tracking__c hist = new History_Tracking__c (Name=dt, Counter__c=1);
            Insert hist;
        } else {
             History_Tracking__c hist = [Select ID,Counter__c FROM History_Tracking__c WHERE createddate = Today and OwnerID =: userID and EventAction__c = 'Visited' and Object__c = 'Opportunity'];
                //History_Tracking__c hist = [Select ID,Counter__c FROM History_Tracking__c WHERE createddate = Today];
                hist.Counter__c = hist.Counter__c + 1;
                update hist;
        }
    }
}
