# Additive Manufacturing Request Power App - Deployment Guide

## Overview

This guide provides step-by-step instructions for deploying the Additive Manufacturing Request Power App in your Microsoft 365 environment. The deployment process includes setting up data sources, configuring security, and establishing the necessary workflows.

## Prerequisites

### Technical Requirements

- Microsoft 365 subscription with Power Platform included
- Power Apps Per User or Per App licenses
- SharePoint Online or Dataverse environment
- Power Automate licenses (for workflow functionality)
- Azure Active Directory for user authentication

### User Permissions

- Environment Administrator or Environment Maker role in Power Platform
- SharePoint Site Collection Administrator (if using SharePoint)
- Power Platform Administrator (for environment-wide settings)

### Software Requirements

- Web browser (Chrome, Edge, Firefox, Safari)
- Power Apps Studio access
- Power Platform CLI (optional, for automated deployment)

## Deployment Steps

### Phase 1: Environment Setup

#### 1.1 Create Power Apps Environment

1. Navigate to [Power Platform Admin Center](https://admin.powerplatform.microsoft.com)
2. Click **Environments** > **New**
3. Configure environment settings:
   - **Name**: "Additive Manufacturing Production"
   - **Type**: Production
   - **Region**: Select appropriate region
   - **Currency**: Select organization currency
   - **Language**: Select primary language
4. Enable **Dataverse** (recommended) or proceed with SharePoint
5. Click **Save**

#### 1.2 Configure Security Groups

1. In Azure AD Admin Center, create security groups:
   - **AM Request Users**: General users who can submit requests
   - **AM Request Approvers**: Users who can approve requests
   - **AM Request Administrators**: Full access to manage the system

2. Assign users to appropriate groups based on their roles

### Phase 2: Data Source Configuration

#### Option A: Dataverse Setup (Recommended)

1. **Create Request Entity**
   ```
   Entity Name: AM Request
   Primary Field: Title
   Fields:
   - Title (Text)
   - Request Type (Choice)
   - Description (Multiple Lines of Text)
   - Business Justification (Multiple Lines of Text)
   - Quantity (Whole Number)
   - Dimensions (Text)
   - Material (Text)
   - Print Process (Text)
   - Priority (Choice)
   - Special Instructions (Multiple Lines of Text)
   - Estimated Cost (Currency)
   - Target Delivery Date (Date Only)
   - Status (Choice)
   - Submitted By (Text)
   - Submitted Date (Date and Time)
   - Department (Text)
   - Requestor Email (Text)
   ```

2. **Create Lookup Entities**
   - Materials Entity
   - Print Processes Entity
   - Request Types Entity
   - Priority Levels Entity

3. **Configure Choice Fields**
   ```
   Status Choices:
   - Submitted
   - In Review
   - Approved
   - In Production
   - Completed
   - Rejected
   
   Priority Choices:
   - Low
   - Normal
   - High
   - Critical
   
   Request Type Choices:
   - Prototype
   - Functional Part
   - Tooling/Jig
   - Replacement Part
   - Research/Testing
   ```

#### Option B: SharePoint Lists Setup

1. **Create Main Request List**
   - Navigate to your SharePoint site
   - Create new list: "AM Requests"
   - Add columns matching the Dataverse schema above
   - Configure column types and validation rules

2. **Create Lookup Lists**
   - Materials List
   - Print Processes List
   - Request Types List
   - Priority Levels List

3. **Configure List Permissions**
   - Grant read access to all users
   - Grant contribute access to request users
   - Grant full control to administrators

### Phase 3: Power App Import and Configuration

#### 3.1 Import Power App

1. Download the Power App package (.msapp file)
2. Open [Power Apps Studio](https://make.powerapps.com)
3. Click **Apps** > **Import canvas app**
4. Upload the .msapp file
5. Configure import settings:
   - Review package content
   - Map data connections to your environment
   - Set up environment variables if needed
6. Click **Import**

#### 3.2 Configure Data Connections

1. Open the imported app in Power Apps Studio
2. Navigate to **Data** panel
3. Update data source connections:
   - For Dataverse: Connect to your environment's Dataverse
   - For SharePoint: Connect to your SharePoint lists
4. Update collection initialization in App.OnStart if needed
5. Test data connectivity

#### 3.3 Update Environment-Specific Settings

1. **Theme Customization** (if needed)
   - Open App.pa.yaml
   - Modify color values in varTheme object
   - Update branding elements

2. **User Information Source**
   - Update varCurrentUser initialization
   - Configure department lookup from Azure AD
   - Set up proper user identification

3. **Business Logic Updates**
   - Adjust cost calculation formulas if needed
   - Update material and process data
   - Configure approval thresholds

### Phase 4: Power Automate Workflow Setup

#### 4.1 Create Approval Workflow

1. Open [Power Automate](https://flow.microsoft.com)
2. Create new flow: "AM Request Approval"
3. Configure trigger: "When an item is created" (SharePoint) or "When a record is created" (Dataverse)
4. Add approval actions:
   - Send approval request to designated approvers
   - Update request status based on approval outcome
   - Send notification to requestor
5. Save and test the flow

#### 4.2 Create Notification Workflows

1. **Status Update Notifications**
   - Trigger on status change
   - Send email to requestor with updated status
   - Include estimated completion date

2. **Manager Approval for High Priority**
   - Trigger on high/critical priority requests
   - Route to manager for additional approval
   - Update status accordingly

#### 4.3 Create Reporting Workflows

1. **Daily Summary Report**
   - Scheduled flow running daily
   - Aggregate request metrics
   - Send summary to manufacturing team

2. **Monthly Analytics Report**
   - Generate monthly analytics
   - Include cost analysis and trends
   - Send to stakeholders

### Phase 5: Security and Permissions

#### 5.1 Configure App Permissions

1. In Power Apps Studio:
   - Navigate to **Share** tab
   - Add security groups created in Phase 1
   - Assign appropriate permission levels:
     - **AM Request Users**: Can use app
     - **AM Request Approvers**: Can use app + workflow permissions
     - **AM Request Administrators**: Can edit app

#### 5.2 Set Up Row-Level Security (if using Dataverse)

1. Create security roles for different user types
2. Configure field-level permissions
3. Set up business units if departmental separation needed
4. Test permissions with different user accounts

#### 5.3 Data Loss Prevention Policies

1. Review organizational DLP policies
2. Ensure the app complies with data handling requirements
3. Configure connector usage policies if needed
4. Set up monitoring and auditing

### Phase 6: Testing and Validation

#### 6.1 Unit Testing

1. **Form Validation Testing**
   - Test all required field validations
   - Verify error message display
   - Test boundary conditions

2. **Cost Calculation Testing**
   - Verify cost calculations with various inputs
   - Test priority multipliers
   - Validate currency formatting

3. **Navigation Testing**
   - Test all screen transitions
   - Verify back button functionality
   - Test deep linking if applicable

#### 6.2 Integration Testing

1. **Data Source Integration**
   - Test create/read operations
   - Verify data consistency
   - Test error handling for connection issues

2. **Workflow Integration**
   - Test approval workflow triggers
   - Verify notification delivery
   - Test status update mechanisms

3. **Security Testing**
   - Test with different user roles
   - Verify permission restrictions
   - Test unauthorized access attempts

#### 6.3 User Acceptance Testing

1. **Recruit Test Users**
   - Include representatives from different departments
   - Include various technical skill levels
   - Get feedback from actual end users

2. **Test Scenarios**
   - Complete request submission process
   - Test search and filtering functionality
   - Verify mobile responsiveness
   - Test accessibility features

3. **Performance Testing**
   - Test with multiple concurrent users
   - Verify load times
   - Test with large datasets

### Phase 7: Go-Live and Monitoring

#### 7.1 Pre-Go-Live Checklist

- [ ] All data sources configured and tested
- [ ] Security permissions set up correctly
- [ ] Workflows tested and activated
- [ ] User training materials prepared
- [ ] Support procedures documented
- [ ] Backup and recovery procedures in place
- [ ] Monitoring and analytics configured

#### 7.2 Go-Live Activities

1. **Announce Launch**
   - Send communication to all users
   - Provide training materials and documentation
   - Set up support channels

2. **Monitor Initial Usage**
   - Track app usage analytics
   - Monitor for errors or issues
   - Gather user feedback

3. **Provide Support**
   - Have technical team available for immediate issues
   - Document common questions and solutions
   - Provide user training sessions if needed

#### 7.3 Post-Go-Live Monitoring

1. **Performance Monitoring**
   - Monitor app performance metrics
   - Track user adoption rates
   - Monitor data growth and storage usage

2. **Error Monitoring**
   - Set up alerts for app errors
   - Monitor workflow failures
   - Track support tickets and issues

3. **Usage Analytics**
   - Track feature usage patterns
   - Identify areas for improvement
   - Gather feedback for future enhancements

## Maintenance and Updates

### Regular Maintenance Tasks

1. **Monthly Tasks**
   - Review app performance metrics
   - Check for connector updates
   - Review and update material/process data
   - Audit user permissions

2. **Quarterly Tasks**
   - Review and update business logic
   - Assess user feedback for improvements
   - Update documentation
   - Review security settings

3. **Annual Tasks**
   - Major feature updates
   - Platform version upgrades
   - Security review and updates
   - User training refreshers

### Update Deployment Process

1. **Development Environment Testing**
   - Test all changes in development environment
   - Perform regression testing
   - Get stakeholder approval

2. **Staging Environment Validation**
   - Deploy to staging environment
   - Perform user acceptance testing
   - Validate with production-like data

3. **Production Deployment**
   - Schedule deployment during maintenance window
   - Deploy with rollback plan ready
   - Monitor for issues post-deployment
   - Communicate changes to users

## Troubleshooting

### Common Deployment Issues

1. **Data Connection Failures**
   - Check authentication and permissions
   - Verify connection string configurations
   - Test with different user accounts

2. **Permission Issues**
   - Review security group memberships
   - Check environment permissions
   - Verify SharePoint/Dataverse permissions

3. **Workflow Failures**
   - Check Power Automate licenses
   - Verify trigger configurations
   - Test with sample data

4. **Performance Issues**
   - Review formula efficiency
   - Check data source performance
   - Monitor concurrent user limits

### Support Contacts

- **Technical Issues**: IT Support Team
- **Business Process Questions**: Manufacturing Team Lead
- **Power Platform Issues**: Power Platform Administrator
- **Emergency Support**: [Emergency Contact Information]

## Appendix

### A. Environment Variables Template

```yaml
Environment Variables:
  - Name: "AM_Environment"
    Value: "Production"
  - Name: "AM_SupportEmail"
    Value: "support@company.com"
  - Name: "AM_ManagerEmail"
    Value: "manufacturing.manager@company.com"
```

### B. Security Groups Template

```yaml
Security Groups:
  - Name: "AM-Request-Users"
    Description: "Users who can submit AM requests"
    Members: "All employees"
  
  - Name: "AM-Request-Approvers"
    Description: "Users who can approve AM requests"
    Members: "Manufacturing supervisors"
  
  - Name: "AM-Request-Admins"
    Description: "Full access to AM request system"
    Members: "IT administrators, Manufacturing managers"
```

### C. Testing Checklist

```yaml
Testing Checklist:
  Functional Testing:
    - [ ] Form submission works correctly
    - [ ] Validation rules function properly
    - [ ] Cost calculation is accurate
    - [ ] Search and filtering work
    - [ ] Status updates function
  
  Security Testing:
    - [ ] User permissions work correctly
    - [ ] Data access is restricted properly
    - [ ] Approval workflows function
  
  Performance Testing:
    - [ ] App loads within acceptable time
    - [ ] Concurrent users supported
    - [ ] Large datasets handled properly
  
  User Experience Testing:
    - [ ] Mobile responsive design works
    - [ ] Accessibility features function
    - [ ] User interface is intuitive
```

For additional support or questions about deployment, please contact the Power Platform team or refer to the Microsoft Power Apps documentation.