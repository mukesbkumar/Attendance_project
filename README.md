# Face Recognition Attendance System

A cloud-based attendance management system that uses AWS services for face recognition, authentication, and data storage. The system allows employees to mark their attendance by capturing their photo, and administrators can view and manage attendance records through a comprehensive dashboard.

## 🚀 Features

- **Face Recognition**: Automated attendance marking using AWS Rekognition
- **Real-time Camera Capture**: Web-based camera interface for capturing employee photos
- **Secure Authentication**: AWS Cognito integration for user authentication
- **Admin Dashboard**: Comprehensive dashboard for viewing and managing attendance records
- **Data Export**: CSV export functionality for attendance records
- **Filtering & Search**: Filter attendance records by employee ID, date, and more
- **Cloud Storage**: Automatic image storage in AWS S3
- **Scalable Architecture**: Serverless architecture using AWS Lambda

## 🏗️ Architecture

The system is built on AWS serverless architecture:

- **AWS Rekognition**: Face detection and matching
- **AWS Cognito**: User authentication and authorization
- **AWS DynamoDB**: Data storage (attendance records and employee information)
- **AWS S3**: Image storage
- **AWS Lambda**: Serverless functions for processing
- **API Gateway**: RESTful API endpoints

## 📁 Project Structure

```
Attendance_System/
├── Landing Page.txt          # Employee attendance marking interface
├── Admin dashboard.txt        # Admin dashboard for viewing attendance
├── Face indexing.txt          # Lambda function for indexing faces
├── Face search and Attendance Logging.txt  # Lambda function for face search and logging
└── README.md                  # This file
```

## 🛠️ Components

### 1. Landing Page (Employee Interface)
- Employee login using AWS Cognito
- Real-time camera access
- Photo capture and upload to S3
- Automatic face recognition processing

### 2. Admin Dashboard
- Admin authentication
- View all attendance records
- Filter by employee ID and date
- Export attendance data to CSV
- View statistics (total employees, today's attendance, etc.)
- Image preview functionality

### 3. Lambda Functions

#### Face Indexing Lambda
- Automatically indexes faces when employee photos are uploaded to S3
- Creates face collection in AWS Rekognition
- Maps faces to employee IDs

#### Face Search and Attendance Logging Lambda
- Processes captured attendance photos
- Searches for matching faces in Rekognition collection
- Logs attendance records to DynamoDB with confidence scores

## 🔧 Setup Instructions

### Prerequisites
- AWS Account with appropriate permissions
- AWS Cognito User Pool configured
- AWS Rekognition Collection created
- DynamoDB tables created (`employees` and `attendance`)
- S3 bucket configured for image storage

### AWS Configuration

#### 1. AWS Cognito Setup
- User Pool ID: `us-east-1_FF0WZi1rt`
- Client ID: `17sc983dt9m5nok97b6v3gjb24`
- Identity Pool ID: `us-east-1:e64d8630-6be6-4a08-9fb1-7e2064cecb19`

#### 2. DynamoDB Tables

**employees Table:**
- Primary Key: `EmployeeID` (String)
- Attributes: `Name`, `Department`, `Email`

**attendance Table:**
- Primary Key: `EmployeeID` (String)
- Sort Key: `TimeStamp` (String)
- Attributes: `Confidence` (Number), `S3Key` (String)

#### 3. S3 Bucket Structure
```
your-bucket/
├── employee-photos/     # Employee reference photos for indexing
└── captures/           # Captured attendance photos
```

#### 4. Lambda Functions

**Face Indexing Lambda:**
- Trigger: S3 event on `employee-photos/` folder
- Collection ID: `attendance-collection`
- Function: Indexes faces to Rekognition collection

**Face Search Lambda:**
- Trigger: S3 event on `captures/` folder
- Function: Searches for face matches and logs attendance

### Deployment Steps

1. **Create Rekognition Collection:**
   ```bash
   aws rekognition create-collection --collection-id attendance-collection
   ```

2. **Deploy Lambda Functions:**
   - Upload `Face indexing.txt` as Lambda function for face indexing
   - Upload `Face search and Attendance Logging.txt` as Lambda function for attendance logging
   - Configure S3 triggers for both functions

3. **Set up S3 Bucket:**
   - Create S3 bucket
   - Configure CORS settings
   - Set up bucket policies for Lambda access

4. **Configure Cognito:**
   - Create user pool and identity pool
   - Set up authentication flows
   - Configure IAM roles with appropriate permissions

5. **Deploy Frontend:**
   - Host `Landing Page.txt` (rename to `.html`) for employee access
   - Host `Admin dashboard.txt` (rename to `.html`) for admin access
   - Update AWS configuration in both files if needed

## 📝 Usage

### For Employees

1. Navigate to the Landing Page
2. Login with your Cognito credentials
3. Click "Start Camera" to activate webcam
4. Click "Capture Photo" to take your photo
5. Click "Upload to S3" to submit attendance
6. System automatically processes the image and records attendance

### For Administrators

1. Navigate to the Admin Dashboard
2. Login with admin credentials
3. View attendance statistics on the dashboard
4. Use filters to search by employee ID or date
5. Export attendance data to CSV
6. View detailed attendance records with confidence scores

## 🔐 Security Features

- AWS Cognito authentication for secure access
- IAM roles with least privilege access
- Secure image storage in S3
- Encrypted data in DynamoDB
- Face match confidence threshold (80% default)

## 📊 Features Overview

### Admin Dashboard Features
- **Statistics Cards**: Total employees, today's attendance, total records, average confidence
- **Filtering**: Filter by employee ID and date
- **Pagination**: Navigate through large datasets
- **Export**: Download attendance data as CSV
- **Image Preview**: Click on images to view full size
- **Real-time Updates**: Refresh to get latest attendance records

### Employee Interface Features
- **Camera Integration**: Direct webcam access
- **Photo Preview**: Preview captured image before upload
- **Status Notifications**: Real-time feedback on operations
- **Secure Upload**: Pre-signed URLs for secure S3 uploads

## 🔄 Workflow

1. **Employee Registration:**
   - Employee photo uploaded to `employee-photos/` folder
   - Face Indexing Lambda indexes the face to Rekognition collection
   - Employee data stored in DynamoDB `employees` table

2. **Attendance Marking:**
   - Employee captures photo via web interface
   - Image uploaded to S3 `captures/` folder
   - Face Search Lambda processes the image
   - System searches for matching face in Rekognition collection
   - If match found (confidence ≥ 80%), attendance logged to DynamoDB

3. **Admin Viewing:**
   - Admin accesses dashboard
   - System loads attendance records from DynamoDB
   - Records displayed with employee details, timestamps, and confidence scores

## 🐛 Troubleshooting

### Common Issues

1. **Camera not working:**
   - Ensure HTTPS is enabled (required for camera access)
   - Check browser permissions for camera access

2. **Authentication errors:**
   - Verify Cognito User Pool and Client ID are correct
   - Check IAM roles and policies

3. **Face recognition not working:**
   - Ensure Rekognition collection exists
   - Verify employee faces are indexed
   - Check Lambda function logs for errors

4. **S3 upload failures:**
   - Verify S3 bucket permissions
   - Check CORS configuration
   - Ensure Lambda function has S3 access

## 📈 Future Enhancements

- Real-time attendance notifications
- Mobile app support
- Advanced reporting and analytics
- Multi-location support
- Integration with payroll systems
- Biometric verification enhancements

## 📄 License

This project is proprietary software. All rights reserved.

## 👥 Support

For issues or questions, please contact the development team.

---

**Note**: This system requires proper AWS configuration and credentials. Ensure all AWS services are properly set up before deployment.

