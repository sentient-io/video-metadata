# VideoMetaDataManager

A Python library for managing video metadata using MySQL database with PyMySQL.

## Features

- MySQL-based storage for video metadata with PyMySQL
- JSON serialization for flexible metadata storage
- Simple CRUD operations (Create, Read, Update)
- Type hints and comprehensive documentation
- Connection pooling and retry mechanism
- Sharded database support

## Installation

### 1. Development Installation
```bash
- git clone https://github.com/sentient-io/video-metadata.git
```
```bash
- cd video-metadata
```
```bash
- pip install -e .
```

### 2. Direct Installation

```bash
pip install git+https://github.com/sentient-io/video-metadata.git

```

## Requirements

- Python 3.6+
- PyMySQL>=1.0.2
- MySQL 5.7+ or compatible (tested with Vitess/Sharded MySQL)

## Usage

```python
from video_metadata_manager import VideoMetaDataManager

# Initialize the manager with your database credentials
manager = VideoMetaDataManager(
    host="your_database_host",
    user="your_username",
    password="your_password",
    db_name="your_database_name",
    port=3306  # default MySQL port
)

# Insert video data
video_id = manager.insert_video_data(
    "video_001",
    {
        "title": "My Video",
        "duration": 120,
        "resolution": "1080p",
        "tags": ["tutorial", "python"]
    }
)
print(f"Inserted video with ID: {video_id}")

# Update metadata
updated_count = manager.update_video_metadata(
    "video_001",
    {
        "title": "Updated Video Title",
        "category": "educational",
        "views": 1500
    }
)
print(f"Updated {updated_count} record(s)")

# Get metadata for a specific video
video_data = manager.get_video_metadata("video_001")
print(f"Video data: {video_data}")

# Get all videos
all_videos = manager.get_all_videos()
print(f"Total videos: {len(all_videos)}")
```

## API Reference

### VideoMetaDataManager

#### `__init__(db_name='video_database', host='localhost', user='root', password='', port=3306)`
Initialize the VideoMetaDataManager with database connection parameters.

#### `insert_video_data(video_id: str, metadata: Dict) -> Optional[int]`
Insert a new video record with the given metadata.
- Returns: The ID of the inserted row, or None if insertion failed

#### `update_video_metadata(video_id: str, new_metadata: Dict) -> int`
Update metadata for an existing video.
- Returns: Number of rows updated (0 if not found, 1 if updated)

#### `get_video_metadata(video_id: str) -> Optional[Dict]`
Retrieve metadata for a specific video.
- Returns: Dictionary containing video data or None if not found

#### `get_all_videos() -> List[Dict]`
Retrieve all video records from the database.
- Returns: List of video dictionaries

## Database Schema

The library creates a `video_metadata` table with:
- `video_id`: VARCHAR(255) PRIMARY KEY (unique video identifier)
- `metadata`: TEXT (JSON string containing video metadata)
- `created_at`: TIMESTAMP (automatically set on record creation)
- `updated_at`: TIMESTAMP (automatically updated on record modification)

## Error Handling

The library provides detailed error messages for common database operations. All database operations are wrapped in try-catch blocks to prevent crashes.

