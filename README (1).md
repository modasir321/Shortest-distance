
# Find my shop:
This project aims to help users find nearby shops based on their location and desired business category. It utilizes geospatial data and distance calculations to identify suitable shops within a specified radius. The program provides the functionality to add new shops to the market, retrieve relevant shops based on user preferences, calculate distances between the user's location and shops, and determine the nearest shop for a specific business category. The project also includes the ability to mark shop locations on a map for visual representation.



## Roadmap:

1.Create a class Shop with attributes latitude, longitude, and specialty.
Create a class Market with an empty shops dictionary.

2.Implement the addShop method in the Market class to add a shop to the shops dictionary.
Implement the retrieveShop method in the Market class to find suitable shops based on the user's location and desired business.

3.Implement the calculateDistance method in the Market class to calculate the distance between two points using the Haversine formula.

4.Implement the markLocationOnMap method in the Market class to open a web browser and mark a location on a map using the provided latitude and longitude.

5.Implement the calculateShortestDistance method in the Market class to find the nearest shop for the desired business based on the user's location.

6.Create an instance of the Market class named market.
Add shops to the market using the addShop method.

7.Prompt the user to enter their latitude and longitude.

8.Call the markLocationOnMap method of market to mark the user's location on the map.
Prompt the user to enter the desired shop.

9.Call the retrieveShop method of market to find suitable shops for the desired business near the user's location and mark them on the map.

10.Call the calculateShortestDistance method of market to find the nearest shop for the desired business and display the distance.

11.End of the program.1


## Deployment:

The code for this is as follows:
import webbrowser
import geopandas as gpd
import heapq


class Shop:
    def _init_(self, latitude, longitude, specialty):
        self.latitude = latitude
        self.longitude = longitude
        self.specialty = specialty


class Market:
    def _init_(self):
        self.shops = {}

    def addShop(self, name, latitude, longitude, specialty):
        shop = Shop(latitude, longitude, specialty)
        self.shops[name] = shop

    def retrieveShop(self, user_latitude, user_longitude, desired_business):
        suitable_shops = []

        for name, shop in self.shops.items():
            if shop.specialty == desired_business:
                distance = self.calculateDistance(user_latitude, user_longitude, shop.latitude, shop.longitude)
                if distance <= 5:  # Considering a radius of 5 kilometers for suitability
                    suitable_shops.append((distance, shop))

        if suitable_shops:
            self.markLocationOnMap(user_latitude, user_longitude)
            for distance, shop in suitable_shops:
                self.markLocationOnMap(shop.latitude, shop.longitude)
                print(f"{shop.specialty} shop located at ({shop.latitude}, {shop.longitude}) is suitable to open near your current location.")
                print(f"Distance: {distance:.2f} km")
        else:
            print("No suitable shops found for the desired business.")

    def calculateDistance(self, lat1, lon1, lat2, lon2):
        # Calculate the distance between two points using the Haversine formula
        from math import radians, sin, cos, sqrt, atan2
        R = 6371  # Radius of the Earth in kilometers
        lat1_rad, lon1_rad, lat2_rad, lon2_rad = map(radians, [lat1, lon1, lat2, lon2])
        dlat = lat2_rad - lat1_rad
        dlon = lon2_rad - lon1_rad
        a = sin(dlat/2)*2 + cos(lat1_rad) * cos(lat2_rad) * sin(dlon/2)*2
        c = 2 * atan2(sqrt(a), sqrt(1-a))
        distance = R * c
        return distance

    def markLocationOnMap(self, latitude, longitude):
        lat_str = str(latitude)
        lon_str = str(longitude)

        # Open the map in the web browser and mark the location
        url = f"https://www.openstreetmap.org/?mlat={lat_str}&mlon={lon_str}#map=17/{lat_str}/{lon_str}"
        webbrowser.open(url)

        print("Location marked successfully.")

    def calculateShortestDistance(self, user_latitude, user_longitude, desired_business):
        distances = {}
        for name, shop in self.shops.items():
            if shop.specialty == desired_business:
                distance = self.calculateDistance(user_latitude, user_longitude, shop.latitude, shop.longitude)
                distances[name] = distance

        if distances:
            shortest_distance = float('inf')
            nearest_shop = None
            for name, distance in distances.items():
                if distance < shortest_distance:
                    shortest_distance = distance
                    nearest_shop = name

            if nearest_shop:
                print(f"The nearest {desired_business} shop is {nearest_shop}")
                print(f"Distance: {shortest_distance:.2f} km")
            else:
                print("No shops found for the desired business.")
        else:
            print("No shops found for the desired business.")



market = Market()
market.addShop("Chezious G -13", 33.6458194, 72.9639667, "Fast food")
market.addShop("Travellers lodge", -23.91341, 29.4483, "Travelling Tickets")
market.addShop("The TecKart(cloth store)", 33.6417524, 72.9598876, "Clothing store")
market.addShop("Devsnic Resthouse ", 33.6476724, 72.9609317, "Tech House")
market.addShop("Hamdan Suites", 33.6484718, 72.9740289, "Clothing store")
market.addShop("Professional Lodges(rest house)", 33.6468019, 72.9699192, "Rest House")
market.addShop("GET cash & carry", 33.6513732, 72.9750115, "Grocery Store")
market.addShop("Rayans Marquee", 33.6505471, 72.9750115, "Marriage Hall")
market.addShop("Homey Islamabad(rest house)", 33.651957, 72.9729129, "Rest House")
market.addShop("Dr. A.Q. Khan School System - Head Office", 33.657831, 72.969077, "Educational Institution")
market.addShop("GB Lodges (G-13 Branch), Islamabad", 33.657246, 72.9656469, "Rest house")
market.addShop("Kashee's Lodges(rest houses)", 33.6529798, 72.9682332, "Rest House")
market.addShop("Umda Horizon(rest houses)", 33.6507349, 72.9699491, "Rest house")
market.addShop("Fatima Group of Girls' Hostels.", 33.6461319, 72.9695185, "Hostles")
market.addShop("Kashmir Avenue Apartments", 33.6447626, 72.9691025, "Rest House")
market.addShop("SU Tech", 33.655547, 72.9641582, "Tech house")
market.addShop("Continental boutique house", 33.6565836, 72.962543, "Clothing Shop")
market.addShop("Kabuli 2 ", 33.6542714, 72.9626706, "Resturant")
market.addShop("Cuisine'de Islamabad", 33.652122, 72.9608894, "Grocery Store")
market.addShop("AYMI Falafel", 33.6538093, 72.9582148, "Resturant")
market.addShop("THE WORKOUTS", 33.6507803, 72.9530899, "The Gym")
market.addShop("Kalsoom ultrasound's corner", 33.6485618, 72.9535318, "The Laboratory")



market.addShop("KHS SPORTS ARENA",33.6741528,72.996432, "Sports Area")
market.addShop("Islamabad College of Arts and Sciences (ICAS)",33.6722855,72.9984023, "College")
market.addShop("Attock Petrol Pump",33.667674,72.9994498, "Petrol Pump")
market.addShop("Nova CSS Academy",33.6780661,73.0162725, "Acadmey")
market.addShop("Gimme Granola",33.6745342,73.002809, "Resturant")
market.addShop("Classic Electronics",33.6751037,73.0021212, "Electronics Shop")
market.addShop("Bakeman",33.6715689,72.9918208, "Bakery")
market.addShop("Jahangir Aman Dental Care",33.6715689,72.9918208, "Dental Clinic")
market.addShop("New Al-Madina Dry clean & Steam Laundry",33.6720976,72.9913595, "Laundry")
market.addShop("Ahmed Milk Centre",33.6720976,72.9913595, "Milk Shop")
market.addShop("Shah G Restaurant & Biryani House",33.6715689,72.9918208, "Biryani Point")
market.addShop("Bismillah Plaza",33.6720976,72.9913595, "Plaza Center")
market.addShop("Ideal Modrate Hair Dresser Beauty Saloon",33.6715689,72.9918208, "Hair Dresser")
market.addShop("Hair Dresser Beauty Saloon",33.67201,72.9908817, "Hair Dresser")
market.addShop("Suleman Khattak General Store",33.6720976,72.9913595, "General Store")
market.addShop("Suleman Khattak General Store",33.6720976,72.9913595, "General Store")
market.addShop("Rice & Rice",33.6720976,72.9913595, "Rice Store")
market.addShop("Ghumman Real Estate and Builders",33.6720976,72.9913595,"Real Estate")
market.addShop("Pak Land Dry Cleaners",33.6720976,72.9913595,"Laundry")
market.addShop("Al Ghaffar Shopping Mall",33.6685625,72.9983906,"Shopping Mall")
market.addShop("Burning Bull",33.669717,72.9980874,"Resturant")
market.addShop("The Bank of Punjab",33.667674,72.9994498,"Bank")
market.addShop("Akram lucky juice",33.6530742,73.2255876,"Juice Corner")
market.addShop("HBL Bank",33.6687625,73.0005781,"Bank")
market.addShop("Blue World City Islamabad",33.5530834,73.1365962,"Real Estate")
market.addShop("Speed Wash",33.6700125,73.0011719,"Car Wash")
market.addShop("Ace One",33.6699772,73.00007769,"Mobile Shop")
market.addShop("Sehberg School System",33.6673007,72.9924551,"School System")
market.addShop("Faran Public School Islamabad",33.6703187,72.9940271,"School System")
market.addShop("Cafe Vintage ",33.6710878,72.9926707,"Cafe")
market.addShop("Safe way Girls Hostel ",33.6696056,72.9948884,"Girls Hostel")
market.addShop("Islamia Group of Girls Hostels, Islamabad. ",33.6681449,72.9932565,"Girls Hostel")

user_latitude = float(input("Enter your latitude: "))
user_longitude = float(input("Enter your longitude: "))

market.markLocationOnMap(user_latitude, user_longitude)

desired_business = input("Enter the desired shop ")

market.retrieveShop(user_latitude, user_longitude, desired_business)

market.calculateShortestDistance(user_latitude, user_longitude, desired_business

## libraries:

webbrowser: This library allows opening a web browser to mark locations on a map.

geopandas: This library is used for geographic data manipulation and analysis. It provides functionality for working with geospatial data, such as latitude and longitude coordinates.

heapq: This library provides heap-based algorithms, which are used in this code for finding the nearest shop based on distance.

These libraries are used to perform specific tasks in the code, such as opening a web browser, calculating distances between coordinates, and manipulating geospatial data.


## Acknowledgements

 We would like to express our sincere gratitude to the creators and developers of Leaflet and Google Maps for their invaluable contributions to this project. The integration of Leaflet and Google Maps has allowed us to incorporate geospatial functionality and seamlessly display coordinate data on interactive maps.
