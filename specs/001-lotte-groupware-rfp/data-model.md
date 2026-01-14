# Data Model

**Source**: [spec.md](./spec.md)

This document outlines the core data entities for the Lotte Groupware system.

---

### 1. User

Represents an employee within the organization. Linked to the central HR system and organization chart.

-   **id**: `UUID` (Primary Key) - Unique identifier for the user.
-   **employeeId**: `String` (Unique) - The official employee number.
-   **name**: `String` - User's full name.
-   **email**: `String` (Unique) - User's corporate email address.
-   **passwordHash**: `String` - Hashed password for system login.
-   **position**: `String` - User's official position (e.g., "대리", "과장").
-   **departmentId**: `UUID` (Foreign Key to Department) - The department the user belongs to.
-   **profileImageUrl**: `String` (Optional) - URL for the user's profile picture.
-   **status**: `Enum` (`ACTIVE`, `INACTIVE`, `ON_LEAVE`) - Current status of the user.
-   **createdAt**: `DateTime` - Timestamp of user creation.
-   **updatedAt**: `DateTime` - Timestamp of last update.

**Relationships**:
-   Belongs to one `Department`.
-   Has many `Posts`, `Approvals` (as originator), `ApprovalSteps` (as approver).

---

### 2. Department

Represents a department or team in the organization chart.

-   **id**: `UUID` (Primary Key)
-   **name**: `String` - Name of the department (e.g., "디지털전략팀").
-   **parentId**: `UUID` (Optional, Foreign Key to Department) - For hierarchical structure.
-   **createdAt**: `DateTime`
-   **updatedAt**: `DateTime`

**Relationships**:
-   Has many `Users`.
-   Can have a parent `Department`.

---

### 3. Approval (전자결재 문서)

Represents a single electronic approval document.

-   **id**: `UUID` (Primary Key)
-   **title**: `String` - Title of the document.
-   **content**: `JSONB` or `Text` - The main content of the document, potentially rich text from an editor.
-   **formId**: `UUID` (Foreign Key to ApprovalForm) - The template/form used for this document.
-   **originatorId**: `UUID` (Foreign Key to User) - The user who created the document.
-   **status**: `Enum` (`DRAFT`, `IN_PROGRESS`, `APPROVED`, `REJECTED`, `CANCELED`) - Current status of the approval process.
-   **documentNumber**: `String` (Unique) - Official document number generated upon completion.
-   **createdAt**: `DateTime`
-   **completedAt**: `DateTime` (Optional) - Timestamp when the approval process concluded.

**Relationships**:
-   Belongs to one `User` (as originator).
-   Has many `ApprovalSteps`.
-   Has many `Attachments`.

---

### 4. ApprovalStep (결재선)

Represents a single step in the approval workflow for a document.

-   **id**: `UUID` (Primary Key)
-   **approvalId**: `UUID` (Foreign Key to Approval) - The document this step belongs to.
-   **approverId**: `UUID` (Foreign Key to User) - The user assigned to this step.
-   **stepOrder**: `Integer` - The order of this step in the workflow (e.g., 1, 2, 3).
-   **status**: `Enum` (`PENDING`, `APPROVED`, `REJECTED`, `SKIPPED`) - Status of this specific step.
-   **type**: `Enum` (`APPROVAL`, `AGREEMENT`) - The type of action required (결재, 합의).
-   **comment**: `Text` (Optional) - Comment left by the approver.
-   **processedAt**: `DateTime` (Optional) - Timestamp when the approver took action.

**Relationships**:
-   Belongs to one `Approval`.
-   Belongs to one `User` (as approver).

---

### 5. Post (게시물)

Represents a post in a bulletin board.

-   **id**: `UUID` (Primary Key)
-   **boardId**: `UUID` (Foreign Key to Board) - The board this post belongs to.
-   **authorId**: `UUID` (Foreign Key to User) - The user who wrote the post.
-   **title**: `String`
-   **content**: `Text`
-   **isNotice**: `Boolean` - Whether this is a notice post (pinned to the top).
-   **viewCount**: `Integer` - Number of times the post has been viewed.
-   **createdAt**: `DateTime`
-   **updatedAt**: `DateTime`

**Relationships**:
-   Belongs to one `Board`.
-   Belongs to one `User` (as author).
-   Has many `Comments`.
-   Has many `Attachments`.

---

### 6. Comment (댓글)

Represents a comment on a post.

-   **id**: `UUID` (Primary Key)
-   **postId**: `UUID` (Foreign Key to Post)
-   **authorId**: `UUID` (Foreign Key to User)
-   **content**: `Text`
-   **createdAt**: `DateTime`
-   **updatedAt**: `DateTime`

**Relationships**:
-   Belongs to one `Post`.
-   Belongs to one `User` (as author).
