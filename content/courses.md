---
title: "Courses"
comments: false
ShowToc: false
ShowReadingTime: false
hideMeta: true
disableShare: true
---

<div class="course-container">
    <div class="course-column">
        <h2>iOS Development Course: SwiftUI and TCA</h2>
        <p>Unlock the full potential of iOS development with comprehensive course on SwiftUI and TCA, designed to equip you with the knowledge and skills needed to create seamless and intuitive user experiences.</p>
        <div class="price">
        <p>Price: $9.99</p>
        </div>
        <button class="buy-now-btn" onclick="redirectToThankYouPage()">Buy Now</button>
        <h3>Features:</h3>
        <ul>
            <li>Project setup</li>
            <li>Best practices</li>
            <li>Automation</li>
            <li>Testing</li>
            <li>SwiftUI</li>
            <li>The Composable Architecture</li>
        </ul>        
    </div>    
</div>

<script>
function redirectToThankYouPage() {
    var currentUrl = window.location.href;    
    var updatedUrl = currentUrl.replace("/courses", ""); 
    window.location.href = updatedUrl + "thank-you";
}
</script>

<style>
.course-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

.course-column {
    width: 100%;        
    border: 1px solid #ddd;
    border-radius: 20px;
    padding: 20px;
    transition: transform 0.3s ease-in-out;
    margin-bottom: 20px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); /* Add box-shadow */
}

.course-column:hover {
    transform: translateY(-5px);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); /* Add box-shadow */
}

.price {
    font-size: 24px;
    font-weight: bold;    
    margin-bottom: 0; /* Adjust margin */
}

h2 {
    margin-bottom: 10px;
}

h3 {
    margin-bottom: 5px;
}

p {
    margin-bottom: 20px;
}

.buy-now-btn {
    background-color: #007bff;
    border: none;
    color: white;
    padding: 15px 20px;
    width: 180px; /* Adjusted width for a slightly wider button */ 
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 18px;
    font-weight: bold;   
    border-radius: 20px;
    cursor: pointer;
    transition: background-color 0.3s;
}

.buy-now-btn:hover {
    background-color: #0056b3;
}

 /* Light mode */
        body.light-mode {
            background-color: #f4f4f4;
            color: #333;
        }
        /* Dark mode */
        body.dark-mode {
            background-color: #333;
            color: #f4f4f4;
        }

</style>
