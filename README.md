package com.example.demo;

import org.springframework.web.bind.annotation.*;

import java.util.ArrayList;
import java.util.List;

@RestController
public class UmbController {
    private List<Book> books;

    public UmbController(){
        this.books = init();
    }

    private List<Book> init(){
        List<Book> books = new ArrayList<>();
        Book book = new Book();
        book.setAuthor("a");
        book.setTitle("b");
        books.add(book);

        Book book2 = new Book();
        book2.setAuthor("x");
        book2.setTitle("y");
        books.add(book2);
        return books;
    }

    @GetMapping("/api/books")
    public List<Book> getBooks(@RequestParam(required = false) String bookAuthor){
        if (bookAuthor == null){
            return this.books;
        }
        List<Book> filteredBooks = new ArrayList<>();

        for(Book book : books){
            if (book.getAuthor().equals(bookAuthor)){
                filteredBooks.add(book);
            }
        }
        return filteredBooks;

    }
    @GetMapping("/api/books/{bookId}")
    public Book getBook(@PathVariable Integer bookId){
        return this.books.get(bookId);

    }
    //@RequestParam(required = false) String lastname
    //@GetMapping("/api/books")
    //public Book queryBook(){

    //}
    @PostMapping("/api/books")
    public Integer createBook(@RequestBody Book book){
        this.books.add(book);

        return this.books.size() -1;
    }
    @DeleteMapping("/api/books{bookId}")
    public void deleteBook(@PathVariable Integer bookId){
        this.books.remove(this.books.get(bookId));

    }
    @PutMapping("/api/books{bookId}")
    public void updateBook(@PathVariable Integer bookId, @RequestBody Book book){
        this.books.get(bookId).setTitle(book.getTitle());
        this.books.get(bookId).setAuthor(book.getAuthor());
    }
}
