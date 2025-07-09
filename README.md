# Additive Manufacturing Request Power App

A Microsoft Power Apps canvas application for submitting and managing additive manufacturing (3D printing) requests in an enterprise environment.

## Overview

This Power App provides a user-friendly interface for employees to submit requests for 3D printed parts and components. The application captures all necessary information for manufacturing teams to process requests efficiently while maintaining proper documentation and workflow management.

## Features

- **Comprehensive Request Form**: Captures all essential information including part specifications, materials, quantities, and business justification
- **File Upload Support**: Allows users to attach CAD files, technical drawings, and reference documents
- **Material Selection**: Guided selection of appropriate 3D printing materials and processes
- **Request Tracking**: Status tracking and notifications throughout the manufacturing process
- **Approval Workflow**: Integration with Power Automate for routing requests through appropriate approval chains
- **Cost Estimation**: Basic cost calculation based on material, volume, and complexity
- **User-Friendly Interface**: Modern, responsive design following Fluent UI principles

## Technical Specifications

- **Platform**: Microsoft Power Apps (Canvas App)
- **Data Storage**: SharePoint Lists / Dataverse
- **File Storage**: SharePoint Document Library
- **Workflow**: Power Automate integration
- **Authentication**: Azure AD / Microsoft 365

## Installation

1. Import the Power App solution file (.msapp)
2. Configure data connections (SharePoint/Dataverse)
3. Set up Power Automate workflows
4. Configure user permissions
5. Deploy to target environment

## Usage

Users can access the app to:
1. Fill out manufacturing request details
2. Upload supporting files (CAD models, drawings)
3. Select materials and printing processes
4. Submit requests for approval
5. Track request status and receive updates

## Development

This app follows Microsoft Power Apps coding standards and best practices:
- Consistent naming conventions
- Proper error handling
- Performance optimization
- Accessibility compliance
- Code documentation

## Support

For technical support or feature requests, please contact the IT department or submit an issue through the designated support channel.
