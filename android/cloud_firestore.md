# Cloud Firestore

Cloud Firestore is a cloud-hosted, NoSQL database that your iOS, Android, and web apps can access directly via native SDKs.

Following Cloud Firestore's NoSQL data model, you store data in **documents** that contain fields mapping to values. These documents are stored in **collections**, which are containers for your documents that you can use to organize your data and build queries.

Each document contains a set of key-value pairs. Cloud Firestore is **optimized for storing large collections of small documents**.

Documents support many different data types, from simple strings and numbers, to complex, nested objects. You can also create subcollections within documents and build hierarchical data structures that scale as your database grows.

**The names of documents within a collection are unique**. You can provide your own keys, such as user IDs, or you can let Cloud Firestore create random IDs for you automatically.

A collection contains documents and nothing else. It can't directly contain raw fields with values, and it can't contain other collections.

Create shallow queries to retrieve data at the document level without needing to retrieve the entire collection, or any nested subcollections. Add sorting, filtering, and limits to your queries or cursors to paginate your results.

Protect access to your data in Cloud Firestore with **Firebase Authentication** and **Cloud Firestore Security Rules**.

## Initialize Cloud Firestore

```kotlin
// Access a Cloud Firestore instance from your Activity
val db = FirebaseFirestore.getInstance()
```

## Add data

Cloud Firestore stores data in **Documents**, which are stored in **Collections**. Cloud Firestore creates collections and documents implicitly the first time you add data to the document. You do not need to explicitly create collections or documents.

```kotlin
// Create a new user with a first and last name
val user = hashMapOf(
        "first" to "Ada",
        "last" to "Lovelace",
        "born" to 1815
)

// Add a new document with a generated ID
db.collection("users")
    .add(user)
    .addOnSuccessListener { documentReference ->
        Log.d(TAG, "DocumentSnapshot added with ID: ${documentReference.id}")
    }
    .addOnFailureListener { e ->
        Log.w(TAG, "Error adding document", e)
    }
```

Now add another document to the users collection. Notice that this document includes a key-value pair (middle name) that does not appear in the first document. Documents in a collection can contain different sets of information.

```kotlin
// Create a new user with a first, middle, and last name
val user = hashMapOf(
        "first" to "Alan",
        "middle" to "Mathison",
        "last" to "Turing",
        "born" to 1912
)

// Add a new document with a generated ID
db.collection("users")
    .add(user)
    .addOnSuccessListener { documentReference ->
        Log.d(TAG, "DocumentSnapshot added with ID: ${documentReference.id}")
    }
    .addOnFailureListener { e ->
        Log.w(TAG, "Error adding document", e)
    }
```

## Read data

```kotlin
db.collection("users")
        .get()
        .addOnSuccessListener { result ->
            for (document in result) {
                Log.d(TAG, "${document.id} => ${document.data}")
            }
        }
        .addOnFailureListener { exception ->
            Log.w(TAG, "Error getting documents.", exception)
        }
```

## Secure your data

If you're using the Web, Android, or iOS SDK, use Firebase Authentication and Cloud Firestore Security Rules to secure your data in Cloud Firestore.

```
// Allow read/write access on all documents to any user signed in to the application
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth.uid != null;
    }
  }
}
```

## Hierarchical data

To understand how hierarchical data structures work in Cloud Firestore, consider an example chat app with messages and chat rooms.
You can create a collection called **rooms** to store different chat rooms.

Now that you have chat rooms, decide how to store your messages. You might **not** want to store them in the chat room's document. **Documents in Cloud Firestore should be lightweight**, and a chat room could contain a large number of messages. However, **you can create additional collections within your chat room's document, as subcollections**.

## Subcollections

A subcollection is a collection associated with a specific document.

You can create a **subcollection called messages** for every room document in your rooms collection:

In this example, you would create a reference to a message in the subcollection with the following code:

```kotlin
val messageRef = db
        .collection("rooms").document("roomA")
        .collection("messages").document("message1")
```

Subcollections allow you to structure data hierarchically, making data easier to access. To get all messages in roomA, **you can create a collection reference to the subcollection messages** and interact with it like you would any other collection reference.

## Order and limit data

By default, a query retrieves all documents that satisfy the query in ascending order by document ID. You can specify the sort order for your data using orderBy(), and you can limit the number of documents retrieved using limit().

```kotlin
citiesRef.orderBy("name").limit(3)
citiesRef.orderBy("name", Query.Direction.DESCENDING).limit(3)
```

You can also order by multiple fields. For example, if you wanted to order by state, and within each state order by population in descending order:

```kotlin
citiesRef.orderBy("state").orderBy("population", Query.Direction.DESCENDING)
```

## Paginate data with query cursors

With query cursors in Cloud Firestore, you can split data returned by a query into batches according to the parameters you define in your query.

Use the **startAt()** or **startAfter()** methods to define the start point for a query. The startAt() method includes the start point, while the startAfter() method excludes it.

```kotlin
// Get all cities with a population >= 1,000,000, ordered by population,
db.collection("cities")
        .orderBy("population")
        .startAt(1000000)
```

Similarly, use the **endAt()** or **endBefore()** methods to define an end point for your query results.

```kotlin
// Get all cities with a population <= 1,000,000, ordered by population,
db.collection("cities")
        .orderBy("population")
        .endAt(1000000)
```

## 참고

https://firebase.google.com/docs/firestore/
