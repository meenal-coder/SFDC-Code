trigger CompetencyCourse_Share on CompetencyCourse__c (after insert) {
    if(Trigger.isInsert){
        /** CompetencyCourse__Share is the sharing object for the CompetencyCourse__c object.
            courses is the list of share table       **/
        List<CompetencyCourse__Share> courses= new List<CompetencyCourse__Share>();
        
        //for Each CompetencyCourse__c object do the following
        
        for(CompetencyCourse__c c:Trigger.New){
            CompetencyCourse__Share employeeCourse= new CompetencyCourse__Share();
            //assigning all the properties of share object with the values as explained earlier
            employeeCourse.parentid=c.id;
            employeeCourse.UserOrGroupId=c.employee__c;
            employeeCourse.accessLevel='edit';
            //EmployeeCourseAccess is the sharing reason created for the CompetencyCourse__c object.
            employeeCourse.rowCause= Schema.CompetencyCourse__Share.rowCause.EmployeeCourseAccess__c;
            //Adding the new created share object to the list defined earlier
            courses.add(employeeCourse);   
        }
        /**inserting the newly created share objects in the database and
        returning in the list of Database.SaveResult which can further use to calculate the no of successful or  failed records **/
        Database.SaveResult[] result= Database.insert(courses,false);
    }   
} 
