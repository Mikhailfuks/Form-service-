package main

import ( "fmt","net/http","html/template"}

)
// Define a struct to hold form data
type ContactForm struct {
 Name  string
 Email string
 Message string
}

func main() {
 // Define a template for the HTML form
 tmpl := template.Must(template.ParseFiles("form.html"))

 // Handle form submissions
 http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
  if r.Method == http.MethodPost {
   // Create a ContactForm instance to hold the form data
   form := ContactForm{}

   // Parse form data from the request
   r.ParseForm()
   form.Name = r.FormValue("name")
   form.Email = r.FormValue("email")
   form.Message = r.FormValue("message")

   // Process the form data (e.g., send an email)
   fmt.Println("Form submitted:", form)
   // ... send email or perform other actions

   // Redirect to a confirmation page or display a success message
   http.Redirect(w, r, "/success", http.StatusFound)
  } else {
   // Display the form
   tmpl.Execute(w, nil)
  }
 })

 // Handle success page
 http.HandleFunc("/success", func(w http.ResponseWriter, r *http.Request) {
  fmt.Fprintln(w, "Form submitted successfully!")
 })

 // Start the server
 fmt.Println("Server listening on port 8080")
 http.ListenAndServe(":8080", nil)
}

// Example form.html template

