# 📡 MindWell API Documentation

## Overview

This document outlines all available API endpoints for the MindWell application. The API follows RESTful principles and uses JSON for request/response bodies.

## Authentication

All protected endpoints require Clerk authentication. Authentication is handled via middleware and session cookies.

### Authentication Headers
```
Authorization: Bearer <clerk_session_token>
```

---

## 🏥 Health Monitoring Endpoints

### Symptoms Management

#### Log a Symptom
```http
POST /api/symptoms
Content-Type: application/json

{
  "name": "ANXIETY",
  "intensity": 7,
  "frequency": "DAILY"
}
```

**Response (200):**
```json
{
  "id": "symptom_123",
  "userId": "user_456",
  "name": "ANXIETY",
  "intensity": 7,
  "frequency": "DAILY",
  "loggedAt": "2024-06-15T10:30:00Z"
}
```

#### Get User Symptoms
```http
GET /api/symptoms?limit=10&offset=0
```

**Response (200):**
```json
{
  "data": [
    {
      "id": "symptom_123",
      "name": "ANXIETY",
      "intensity": 7,
      "frequency": "DAILY",
      "loggedAt": "2024-06-15T10:30:00Z"
    }
  ],
  "total": 15,
  "limit": 10,
  "offset": 0
}
```

#### Get Symptom Details
```http
GET /api/symptoms/:id
```

**Response (200):**
```json
{
  "id": "symptom_123",
  "userId": "user_456",
  "name": "ANXIETY",
  "intensity": 7,
  "frequency": "DAILY",
  "loggedAt": "2024-06-15T10:30:00Z"
}
```

#### Delete Symptom
```http
DELETE /api/symptoms/:id
```

**Response (200):**
```json
{
  "message": "Symptom deleted successfully",
  "id": "symptom_123"
}
```

---

### Medications Management

#### Add Medication
```http
POST /api/medications
Content-Type: application/json

{
  "name": "Sertraline",
  "dosage": "50mg",
  "purpose": "Anxiety management",
  "frequency": "DAILY",
  "adherence": "ALWAYS",
  "startDate": "2024-01-15"
}
```

**Response (201):**
```json
{
  "id": "med_789",
  "userId": "user_456",
  "name": "Sertraline",
  "dosage": "50mg",
  "purpose": "Anxiety management",
  "frequency": "DAILY",
  "adherence": "ALWAYS",
  "startDate": "2024-01-15"
}
```

#### Get User Medications
```http
GET /api/medications
```

**Response (200):**
```json
{
  "data": [
    {
      "id": "med_789",
      "name": "Sertraline",
      "dosage": "50mg",
      "purpose": "Anxiety management",
      "frequency": "DAILY",
      "adherence": "ALWAYS"
    }
  ]
}
```

#### Update Medication Adherence
```http
PUT /api/medications/:id
Content-Type: application/json

{
  "adherence": "SOMETIMES"
}
```

**Response (200):**
```json
{
  "id": "med_789",
  "adherence": "SOMETIMES",
  "message": "Medication updated successfully"
}
```

#### Delete Medication
```http
DELETE /api/medications/:id
```

**Response (200):**
```json
{
  "message": "Medication deleted successfully",
  "id": "med_789"
}
```

---

### Wellness Check-in

#### Log Wellness Status
```http
POST /api/wellness
Content-Type: application/json

{
  "mood": "HAPPY",
  "happiness": 8,
  "sleep": "GOOD",
  "stress": "SLIGHTLY",
  "anxiety": "Feeling good today"
}
```

**Response (201):**
```json
{
  "id": "wellness_101",
  "userId": "user_456",
  "mood": "HAPPY",
  "happiness": 8,
  "sleep": "GOOD",
  "stress": "SLIGHTLY",
  "anxiety": "Feeling good today",
  "createdAt": "2024-06-15T14:20:00Z"
}
```

#### Get Wellness History
```http
GET /api/wellness?days=30
```

**Response (200):**
```json
{
  "data": [
    {
      "id": "wellness_101",
      "mood": "HAPPY",
      "happiness": 8,
      "sleep": "GOOD",
      "stress": "SLIGHTLY",
      "createdAt": "2024-06-15T14:20:00Z"
    }
  ],
  "averageMood": 7.5,
  "averageHappiness": 7.8
}
```

---

## 🤖 AI Chat Endpoints

#### Send Message to AI
```http
POST /api/chat
Content-Type: application/json

{
  "message": "How can I manage my anxiety naturally?"
}
```

**Response (200):**
```json
{
  "role": "model",
  "content": "Here are some natural ways to manage anxiety...",
  "id": "msg_202",
  "createdAt": "2024-06-15T15:45:00Z"
}
```

#### Get Chat History
```http
GET /api/chat?limit=50
```

**Response (200):**
```json
{
  "data": [
    {
      "id": "msg_202",
      "role": "user",
      "content": "How can I manage my anxiety?",
      "createdAt": "2024-06-15T15:44:00Z"
    },
    {
      "id": "msg_203",
      "role": "model",
      "content": "Here are some suggestions...",
      "createdAt": "2024-06-15T15:45:00Z"
    }
  ]
}
```

#### Clear Chat History
```http
DELETE /api/chat
```

**Response (200):**
```json
{
  "message": "Chat history cleared successfully"
}
```

---

## 🎯 Activity/Habit Tracking Endpoints

#### Create Activity
```http
POST /api/activities
Content-Type: application/json

{
  "name": "Morning Meditation",
  "description": "10 minutes of daily meditation",
  "colorCode": "#FF6B6B"
}
```

**Response (201):**
```json
{
  "id": "activity_301",
  "userId": "user_456",
  "name": "Morning Meditation",
  "description": "10 minutes of daily meditation",
  "colorCode": "#FF6B6B",
  "createdAt": "2024-06-15T08:00:00Z"
}
```

#### Get User Activities
```http
GET /api/activities
```

**Response (200):**
```json
{
  "data": [
    {
      "id": "activity_301",
      "name": "Morning Meditation",
      "description": "10 minutes of daily meditation",
      "colorCode": "#FF6B6B",
      "createdAt": "2024-06-15T08:00:00Z"
    }
  ]
}
```

#### Log Activity
```http
POST /api/activities/:id/log
Content-Type: application/json

{
  "count": 1,
  "date": "2024-06-15"
}
```

**Response (201):**
```json
{
  "id": "activityLog_401",
  "activityId": "activity_301",
  "date": "2024-06-15",
  "count": 1,
  "message": "Activity logged successfully"
}
```

#### Get Activity Logs
```http
GET /api/activities/:id/logs?startDate=2024-06-01&endDate=2024-06-30
```

**Response (200):**
```json
{
  "activityId": "activity_301",
  "logs": [
    {
      "id": "activityLog_401",
      "date": "2024-06-15",
      "count": 1
    }
  ],
  "stats": {
    "totalLogs": 15,
    "currentStreak": 5,
    "longestStreak": 12,
    "averageCount": 1.3
  }
}
```

#### Get Activity Stats
```http
GET /api/activities/:id/stats
```

**Response (200):**
```json
{
  "activityId": "activity_301",
  "name": "Morning Meditation",
  "currentStreak": 5,
  "longestStreak": 12,
  "totalLogs": 15,
  "thisWeek": 6,
  "thisMonth": 25
}
```

#### Update Activity
```http
PUT /api/activities/:id
Content-Type: application/json

{
  "name": "Morning Meditation (Updated)",
  "colorCode": "#4ECDC4"
}
```

**Response (200):**
```json
{
  "id": "activity_301",
  "name": "Morning Meditation (Updated)",
  "colorCode": "#4ECDC4",
  "message": "Activity updated successfully"
}
```

#### Delete Activity
```http
DELETE /api/activities/:id
```

**Response (200):**
```json
{
  "message": "Activity deleted successfully",
  "id": "activity_301"
}
```

---

## 👤 User Profile Endpoints

#### Get User Profile
```http
GET /api/user/profile
```

**Response (200):**
```json
{
  "id": "user_456",
  "email": "user@example.com",
  "firstName": "John",
  "lastName": "Doe",
  "age": 28,
  "gender": "Male",
  "height": 180,
  "weight": 75,
  "bloodGroup": "O+",
  "medicalIssues": "Mild anxiety",
  "createdAt": "2024-01-15T10:00:00Z"
}
```

#### Update User Profile
```http
PUT /api/user/profile
Content-Type: application/json

{
  "age": 29,
  "weight": 73,
  "medicalIssues": "Managed anxiety"
}
```

**Response (200):**
```json
{
  "message": "Profile updated successfully",
  "user": {
    "id": "user_456",
    "age": 29,
    "weight": 73,
    "medicalIssues": "Managed anxiety"
  }
}
```

#### Delete User Account
```http
DELETE /api/user/profile
```

**Response (200):**
```json
{
  "message": "Account deleted successfully. All associated data has been removed."
}
```

---

## 📊 Dashboard Analytics Endpoints

#### Get Dashboard Overview
```http
GET /api/dashboard/overview?dateRange=30
```

**Response (200):**
```json
{
  "totalActivities": 15,
  "currentStreak": 7,
  "averageMood": 7.2,
  "medicalIssues": 3,
  "medications": 2,
  "thisWeekLogs": 20
}
```

#### Get Health Status
```http
GET /api/dashboard/health-status
```

**Response (200):**
```json
{
  "symptoms": {
    "total": 8,
    "recent": "ANXIETY, STRESS"
  },
  "medications": {
    "total": 2,
    "adherenceRate": 85
  },
  "wellness": {
    "avgMood": 7.2,
    "avgHappiness": 7.5,
    "avgSleep": "GOOD"
  }
}
```

#### Get Activity Heatmap Data
```http
GET /api/dashboard/activity-heatmap
```

**Response (200):**
```json
{
  "data": [
    {
      "date": "2024-06-15",
      "count": 5,
      "level": "high"
    }
  ]
}
```

---

## 🛡️ Error Responses

### Unauthorized (401)
```json
{
  "error": "Unauthorized",
  "message": "You must be logged in to access this resource",
  "statusCode": 401
}
```

### Forbidden (403)
```json
{
  "error": "Forbidden",
  "message": "You don't have permission to access this resource",
  "statusCode": 403
}
```

### Not Found (404)
```json
{
  "error": "Not Found",
  "message": "The requested resource was not found",
  "statusCode": 404
}
```

### Validation Error (422)
```json
{
  "error": "Validation Error",
  "message": "Invalid input provided",
  "details": {
    "intensity": "Must be between 1 and 10"
  },
  "statusCode": 422
}
```

### Server Error (500)
```json
{
  "error": "Internal Server Error",
  "message": "An unexpected error occurred",
  "statusCode": 500
}
```

---

## 📊 Status Codes Reference

| Status | Meaning |
|--------|---------|
| 200 | OK - Request successful |
| 201 | Created - Resource created successfully |
| 204 | No Content - Request successful, no response body |
| 400 | Bad Request - Invalid request format |
| 401 | Unauthorized - Authentication required |
| 403 | Forbidden - Insufficient permissions |
| 404 | Not Found - Resource not found |
| 422 | Unprocessable Entity - Validation failed |
| 500 | Internal Server Error - Server error |

---

## 🔄 Rate Limiting

- **Free Tier:** 100 requests per hour
- **Premium Tier:** 1000 requests per hour

Rate limit information is included in response headers:
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1623798000
```

---

## 📝 Pagination

For list endpoints, use these query parameters:
```http
GET /api/symptoms?limit=20&offset=0&sort=createdAt&order=desc
```

**Parameters:**
- `limit`: Number of results (max 100, default 20)
- `offset`: Number of results to skip (default 0)
- `sort`: Field to sort by
- `order`: Sort order (asc/desc)

---

## 🔒 CORS Policy

The API supports CORS requests from:
- `http://localhost:3000` (development)
- `https://mindwell.app` (production)

---

## 📚 SDK & Client Libraries

Currently, the API is accessed directly via HTTP. SDKs for popular languages coming soon.

---

## 🆘 Support

For API issues or questions:
- GitHub Issues: [Report a bug](https://github.com/Jameers-code/mindwell/issues)
- Email: support@mindwell.app

