public class TimezoneHelper {

    private static Map<String, TimezoneWrapper> timezonesById;
    private static List<SelectOption> selOptions;
    
    private class TimezoneWrapper {
        private String id;
        private String label;
    }
    
    static {
        
        TimezoneHelper.timezonesById = new Map<String, TimezoneWrapper>();
        Map<String, TimezoneWrapper> timezonesByLabel = new Map<String, TimezoneWrapper>();
        List<String> labels = new List<String>();
        List<Schema.PicklistEntry> ple = Schema.sObjectType.User.fields.TimeZoneSidKey.getPicklistValues();
        
        for (Schema.PicklistEntry p : ple) {
            
            TimezoneWrapper tzw = new TimezoneWrapper();
            tzw.id = p.getValue();
            tzw.label = p.getLabel();
            TimezoneHelper.timezonesById.put(tzw.id, tzw);
            timezonesByLabel.put(tzw.label, tzw);
            labels.add(tzw.label);
        }
        
        labels.sort();
        TimezoneHelper.selOptions = new List<SelectOption>();
        
        for (String label : labels) {
            
            TimezoneWrapper tzw = timezonesByLabel.get(label);
            TimezoneHelper.selOptions.add(new SelectOption(tzw.id, tzw.label));
        }
    }
    
    public static String getCurrentUserTimezone() {
        return UserInfo.getTimeZone().getId();
	}
    
    public static List<SelectOption> getSelectOptions() {
        return TimezoneHelper.selOptions;
	}
    
    public static String getLabel(String id) {
        return TimezoneHelper.timezonesById.get(id).label;
    }
    
    public static DateTime newInstanceDateTime(Integer year, Integer month, Integer day, Integer hour, Integer minute, Integer second, String timezoneId) {
        if (TimezoneHelper.timezonesById.containsKey(timezoneId)) {
            DateTime dt = DateTime.newInstanceGmt(year, month, day, hour, minute, second);
        	return dt.addSeconds(-Timezone.getTimeZone(timezoneId).getOffset(dt)/1000);
        } else {
            return DateTime.newInstance(year, month, day, hour, minute, second);
        }
    }
}
