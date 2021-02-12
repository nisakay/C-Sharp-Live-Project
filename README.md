# C-Sharp-Live-Project

# Introduction
For the last two weeks of my time at the tech academy, I worked with my peers in a team developing a  Web Application built using ASP .Net MVC and Entity Framework. The project is an interactive website for managing the content and productions for a theater/acting company. It's meant to be a content management service (aka CMS) for users who are not technically saavy and want to easily manage what displays in their website. It's also meant to help manage login capability for subscribers, and maintain a wiki of past performances and performers. Working on a this project was a great learning oppertunity for fixing bugs, cleaning up code, and adding requested features. There were some big changes that could have been a large time sink, but we used what we had to deliver what was needed on time. I saw how a good developer works with what they have to make a quality product. I worked on several back end stories that I am very proud of. Because much of the site had already been built, there were also a good deal of front end stories and UX improvements that needed to be completed, all of varying degrees of difficulty. Everyone on the team had a chance to work on front end and back end stories. Over the two week sprint I also had the opportunity to work on some other project management and team programming skills that I'm confident I will use again and again on future projects.

Below are descriptions of the stories I worked on, along with code snippets and navigation links. I also have some full code files in this repo for the larger functionalities I implemented.

# Back End Stories

I was asked to create an entity model for the Production class so that productions can be saved to the database. First, I needed to create a model, and then create a CRUD pages for it.  
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.ComponentModel.DataAnnotations;
using System.Drawing;
using System.ComponentModel.DataAnnotations.Schema;
using System.Runtime.Serialization;

namespace TheatreCMS2.Models
{
    public class ProductionModel
    {
        [Key]
        public int ProductionId { get; set; }       // production primary key
        [Required]
        public string Title { get; set; }           // production title
        public string Playwright { get; set; }      // production playwright
        public string Description { get; set; }     // production description

        [Display(Name = "Opening Day")]
        public DateTime OpeningDay { get; set; }    // production opening day

        [Display(Name = "Closing Day")]
        public DateTime ClosingDay { get; set; }    // production closing day

        //[Display(Name = "Promo Photo")]
        //public virtual ProductionPhoto DefaultPhoto { get; set; }

        [DataType(DataType.Time)]
        [DisplayFormat(DataFormatString = "{0:HH:mm}", ApplyFormatInEditMode = true)]
        [Display(Name = "Evening Showtime")]
        public DateTime ShowtimeEve { get; set; }  // production evening showtime

        [DataType(DataType.Time)]
        [DisplayFormat(DataFormatString = "{0:HH:mm}", ApplyFormatInEditMode = true)]
        [Display(Name = "Matinee Showtime")]
        public DateTime? ShowtimeMat { get; set; }  // production matinee showtime

        [Display(Name = "Ticket Link")]
        public string TicketLink { get; set; }      // url for purchasing tickets
        public int Season { get; set; }             // production season number

        [Display(Name = "World Premiere")]
        public bool IsCurrent { get; set; }   // first time produced in the world

        
    }
}
```
In this story, I was asked to to seed the databse with  15 mock entries in the productions table. 
```
namespace TheatreCMS2.Migrations
{
    using System;
    using TheatreCMS2.Models;
    using System.Data.Entity;
    using System.Data.Entity.Migrations;
    using System.Linq;

    internal sealed class Configuration : DbMigrationsConfiguration<TheatreCMS2.Models.ApplicationDbContext>
    {
        public Configuration()
        {
            AutomaticMigrationDataLossAllowed = true;
            AutomaticMigrationsEnabled = true;
        }

        protected override void Seed(TheatreCMS2.Models.ApplicationDbContext context)
        {
            context.Productions.AddOrUpdate(x => x.ProductionId,
                new ProductionModel() { ProductionId = 1, Title = "To Kill a Mockingbird", Playwright = "Harper Lee’", Description= "Inspired by Lee’s own childhood in Alabama, To Kill a Mockingbird features one of literature’s towering symbols of integrity and righteousness in the character of Atticus Finch, based on Lee’s own father. The character of Scout, based on Lee herself, has come to define youthful innocence—and its inevitable loss—for generation after generation of readers around the world.", 
                    OpeningDay = new DateTime(2021, 02, 05, 15, 30, 00),
                    ShowtimeEve = new DateTime(2021, 03, 10, 19, 30, 00) , 
                    ShowtimeMat = new DateTime(2021, 03, 10, 10, 30, 00), 
                    TicketLink = "ticketlink.com", 
                    Season = 20, 
                    IsCurrent = true 
                },
                new ProductionModel() { ProductionId = 2, Title = "Hamilton" , Playwright = "Richards Rodgers", Description = "Hamilton is the story of the unlikely Founding Father determined to make his mark on the new nation as hungry and ambitious as he is. From bastard orphan to Washington's right-hand man, rebel to war hero, a loving husband caught in the country's first sex scandal, to the Treasury head who made an untrusting world believe in the American economy. George Washington, Eliza Hamilton, Thomas Jefferson and Hamilton's lifelong friend/foil Aaron Burr all make their mark in this astonishing new musical exploration of a political mastermind." ,
                    OpeningDay = new DateTime(2021, 02, 12, 14, 30, 00),
                    ShowtimeEve = new DateTime(2021, 03, 16, 20, 30, 00),
                    ShowtimeMat = new DateTime(2021, 03, 11, 10, 30, 00),
                    TicketLink = "ticketlink.com",
                    Season = 1,
                    IsCurrent = true
                },
                new ProductionModel() { ProductionId = 3, Title = "Wicked" , Playwright = "Gershwin", Description = "Wicked, the Broadway sensation, looks at what happened in the Land of Oz…but from a different angle. Long before Dorothy arrives, there is another girl, born with emerald-green skin—smart, fiery, misunderstood, and possessing an extraordinary talent. When she meets a bubbly blonde who is exceptionally popular, their initial rivalry turns into the unlikeliest of friendships…until the world decides to call one “good,” and the other one “wicked.”",
                    OpeningDay = new DateTime(2021, 02, 05, 15, 30, 00),
                    ShowtimeEve = new DateTime(2021, 04, 10, 21, 30, 00),
                    ShowtimeMat = new DateTime(2021, 05, 10, 12, 30, 00),
                    TicketLink = "ticketlink.com",
                    Season = 5,
                    IsCurrent = true
                },
                new ProductionModel() { ProductionId = 4, Title = "Moulin Rouge! The Musical" , Playwright = "Al Hirschfeld", Description = "Enter a world of splendor and romance, of eye-popping excess, of glitz, grandeur and glory! A world where Bohemians and aristocrats rub elbows and revel in electrifying enchantment. Pop the champagne and prepare for the spectacular spectacular... Welcome to Moulin Rouge! The Musical.",
                    OpeningDay = new DateTime(2021, 02, 05, 15, 30, 00),
                    ShowtimeEve = new DateTime(2021, 03, 10, 19, 30, 00),
                    ShowtimeMat = new DateTime(2021, 03, 10, 10, 30, 00),
                    TicketLink = "ticketlink.com",
                    Season = 20,
                    IsCurrent = true
                },
                new ProductionModel() { ProductionId = 5, Title = "The Phantom of the Opera" , Playwright = "Majestic", Description = "Based on the 1910 horror novel by Gaston Leroux, which has been adapted into countless films, The Phantom of the Opera follows a deformed composer who haunts the grand Paris Opera House. Sheltered from the outside world in an underground cavern, the lonely, romantic man tutors and composes operas for Christine, a gorgeous young soprano star-to-be. As Christine’s star rises, and a handsome suitor from her past enters the picture, the Phantom grows mad, terrorizing the opera house owners and company with his murderous ways. Still, Christine finds herself drawn to the mystery man.",
                    OpeningDay = new DateTime(2021, 02, 05, 15, 30, 00),
                    ShowtimeEve = new DateTime(2021, 03, 10, 19, 30, 00),
                    ShowtimeMat = new DateTime(2021, 03, 10, 10, 30, 00),
                    TicketLink = "ticketlink.com",
                    Season = 20,
                    IsCurrent = true
                },
                new ProductionModel() { ProductionId = 6, Title = "The Lion King" , Playwright = "Minskoff", Description = "A lively stage adaptation of the Academy Award-winning 1994 Disney film, The Lion King is the story of a young lion prince living in the flourishing African Pride Lands.When an unthinkable tragedy, orchestrated by Simba’s wicked uncle, Scar, takes his father’s life, Simba flees the Pride Lands, leaving his loss and the life he knew behind. Eventually companioned by two hilarious and unlikely friends, Simba starts anew. But when weight of responsibility and a desperate plea from the now ravaged Pride Lands come to find the adult prince, Simba must take on a formidable enemy, and fulfill his destiny to be king.",
                    OpeningDay = new DateTime(2021, 02, 05, 15, 30, 00),
                    ShowtimeEve = new DateTime(2021, 03, 10, 19, 30, 00),
                    ShowtimeMat = new DateTime(2021, 03, 10, 10, 30, 00),
                    TicketLink = "ticketlink.com",
                    Season = 20,
                    IsCurrent = true
                },
                new ProductionModel() { ProductionId = 7, Title = "The Book of Mormon" , Playwright = "Eugene O'Neil", Description = "The Book of Mormon follows two young missionaries who are sent to Uganda to try to convert citizens to the Mormon religion. One missionary, Elder Price, is an enthusiastic go-getter with a strong dedication to his faith, while his partner, Elder Cunningham, is a socially awkward but well meaning nerd whose tendency to embroider the truth soon lands him in trouble. Upon their arrival in Africa, Elders Price and Cunningham learn that in a society plagued by AIDS, poverty and violence, a successful mission may not be as easy as they expected.",
                    OpeningDay = new DateTime(2021, 02, 05, 15, 30, 00),
                    ShowtimeEve = new DateTime(2021, 03, 10, 19, 30, 00),
                    ShowtimeMat = new DateTime(2021, 03, 10, 10, 30, 00),
                    TicketLink = "ticketlink.com",
                    Season = 20,
                    IsCurrent = true
                },
                new ProductionModel() { ProductionId = 8, Title = "Ain't Too Proud – The Life and Times of The Temptations" , Playwright = "Imperial", Description = "Ain't Too Proud is the electrifying new musical that follows The Temptations' extraordinary journey from the streets of Detroit to the Rock & Roll Hall of Fame. With their signature dance moves and unmistakable harmonies, they rose to the top of the charts creating an amazing 42 Top Ten Hits with 14 reaching number one.The rest is history — how they met, the groundbreaking heights they hit and how personal and political conflicts threatened to tear the group apart as the United States fell into civil unrest. This thrilling story of brotherhood, family, loyalty and betrayal is set to the beat of the group's treasured hits, including “My Girl,” “Just My Imagination,” “Get Ready,” “Papa Was a Rolling Stone” and so many more.",
                    OpeningDay = new DateTime(2021, 02, 05, 15, 30, 00),
                    ShowtimeEve = new DateTime(2021, 03, 10, 19, 30, 00),
                    ShowtimeMat = new DateTime(2021, 03, 10, 10, 30, 00),
                    TicketLink = "ticketlink.com",
                    Season = 20,
                    IsCurrent = true
                },
                new ProductionModel() { ProductionId = 9, Title = "Aladdin" , Playwright = "New Amsterdam", Description = "In the middle-eastern town of Agrabah, Princess Jasmine is feeling hemmed in by her father’s desire to find her a royal groom. Meanwhile, the Sultan’s right-hand man, Jafar, is plotting to take over the throne. When Jasmine sneaks out of the palace incognito, she forms an instant connection with Aladdin, a charming street urchin and reformed thief.",
                    OpeningDay = new DateTime(2021, 02, 05, 15, 30, 00),
                    ShowtimeEve = new DateTime(2021, 03, 10, 19, 30, 00),
                    ShowtimeMat = new DateTime(2021, 03, 10, 10, 30, 00),
                    TicketLink = "ticketlink.com",
                    Season = 20,
                    IsCurrent = true
                },
                new ProductionModel() { ProductionId = 10, Title = "Tina: The Tina Turner Musical" , Playwright = "Lunt-Fontane", Description = "Tina follows Tina Turner from her humble beginnings in Nutbush, Tennessee, to her transformation into the global queen of rock 'n' roll. Born Anna Mae Bullock in 1939, Turner rose to fame in the 1960s alongside her husband Ike. She later revealed in her autobiography that she had suffered domestic abuse at his hands—they separated in 1976 and divorced two years later. Turner later made a massive comeback in the 1980s. The Queen of Rock ‘n’ Roll has sold 180 million records worldwide and been honored with 11 Grammy Awards.",
                    OpeningDay = new DateTime(2021, 02, 05, 15, 30, 00),
                    ShowtimeEve = new DateTime(2021, 03, 10, 19, 30, 00),
                    ShowtimeMat = new DateTime(2021, 03, 10, 10, 30, 00),
                    TicketLink = "ticketlink.com",
                    Season = 20,
                    IsCurrent = true
                },
                new ProductionModel() { ProductionId = 11, Title = "Come From Away" , Playwright = "Schoenfeld", Description = "Come From Away is based on the true story of when the isolated community of Gander, Newfoundland played host to the world. What started as an average day in a small town turned in to an international sleep-over when 38 planes, carrying thousands of people from across the globe, were diverted to Gander’s air strip on September 11, 2001. Undaunted by culture clashes and language barriers, the people of Gander cheered the stranded travelers with music, an open bar and the recognition that we’re all part of a global family.",
                    OpeningDay = new DateTime(2021, 02, 05, 15, 30, 00),
                    ShowtimeEve = new DateTime(2021, 03, 10, 19, 30, 00),
                    ShowtimeMat = new DateTime(2021, 03, 10, 10, 30, 00),
                    TicketLink = "ticketlink.com",
                    Season = 20,
                    IsCurrent = true
                },
                new ProductionModel() { ProductionId = 12, Title = "Hadestown" , Playwright = "Walter Kerr", Description = "Welcome to Hadestown, where a song can change your fate. This acclaimed new musical by celebrated singer-songwriter Anaïs Mitchell and innovative director Rachel Chavkin (Natasha, Pierre & The Great Comet of 1812) is a love story for today… and always.",
                    OpeningDay = new DateTime(2021, 02, 05, 15, 30, 00),
                    ShowtimeEve = new DateTime(2021, 03, 10, 19, 30, 00),
                    ShowtimeMat = new DateTime(2021, 03, 10, 10, 30, 00),
                    TicketLink = "ticketlink.com",
                    Season = 20,
                    IsCurrent = true
                },
                new ProductionModel() { ProductionId = 13, Title = "Dear Evan Hansen" , Playwright = "Music Box", Description = "A letter that was never meant to be seen, a lie that was never meant to be told, a life he never dreamed he could have. Evan Hansen is about to get the one thing he’s always wanted: A chance to finally fit in.",
                    OpeningDay = new DateTime(2021, 02, 05, 15, 30, 00),
                    ShowtimeEve = new DateTime(2021, 03, 10, 19, 30, 00),
                    ShowtimeMat = new DateTime(2021, 03, 10, 10, 30, 00),
                    TicketLink = "ticketlink.com",
                    Season = 20,
                    IsCurrent = true
                },
                new ProductionModel() { ProductionId = 14, Title = "Chicago" , Playwright = "Ambassador", Description = "Set in the legendary city during the roaring “jazz hot” 20s, Chicago tells the story of two rival vaudevillian murderesses locked up in Cook County Jail. Nightclub star Velma’s serving time for killing her husband and sister after finding the two in bed together. Driven chorus girl Roxie’s been tossed in the joint for bumping off the lover she’s been cheating on her husband with. Not one to rest on her laurels, Velma enlists the help of prison matron Mama Morton and slickster lawyer Billy Flynn, who turn Velma’s incarceration into a murder-of-the-week media frenzy, thus preparing the world for a splashy showbiz comeback. But Roxie’s got some of her own tricks up her sleeve",
                    OpeningDay = new DateTime(2021, 02, 05, 15, 30, 00),
                    ShowtimeEve = new DateTime(2021, 03, 10, 19, 30, 00),
                    ShowtimeMat = new DateTime(2021, 03, 10, 10, 30, 00),
                    TicketLink = "ticketlink.com",
                    Season = 20,
                    IsCurrent = true
                },
                new ProductionModel() { ProductionId = 15, Title = "Mrs. Doubtfire" , Playwright = "Stephen Sondheim", Description = "Daniel Hillard, a struggling, out-of-work actor, will do anything for his kids. After losing custody in a messy divorce, he disguises himself as Scottish nanny Euphegenia Doubtfire in a desperate attempt to stay in their lives. As his new persona begins to take on a life of her own, Mrs. Doubtfire teaches Daniel more than he bargained for about how to be a father. A hysterical and heartfelt story about holding onto your loved ones against all odds, Mrs. Doubtfire is the next big musical comedy for families — of all kinds.",
                    OpeningDay = new DateTime(2021, 02, 05, 15, 30, 00),
                    ShowtimeEve = new DateTime(2021, 03, 10, 19, 30, 00),
                    ShowtimeMat = new DateTime(2021, 03, 10, 10, 30, 00),
                    TicketLink = "ticketlink.com",
                    Season = 20,
                    IsCurrent = true
                }
                );

            
        }
    }
}
```

# Front-End

For this story I was asked to create a reusable Card for the website so we can keep any Cards we use consistent across the site.

```
<div class="card" style="width: 18rem;">
  <img class="card-img-top" src="https://pyxis.nymag.com/v1/imgs/978/4d0/4b4779e1dcb86984abe55c08366f9babe7-13-empty-theater.rsquare.w700.jpg" alt="Card image cap">
  <div class="card-body">
    <p class="card-text">Theatre Vertigo</p>
  </div>
</div>
```

# Other Skills Learned
Working with a group of developers to identify front and back end bugs to the improve usability of an application
Improving project flow by communicating about who needs to check out which files for their current story
Learning new efficiencies from other developers by observing their workflow and asking questions
Practice with team programming/pair programming when one developer runs into a bug they cannot solve
