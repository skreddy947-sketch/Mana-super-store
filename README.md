# Mana-super-store
Grocery app
   import React, { useState } from 'react';
   import { NavigationContainer } from '@react-navigation/native';
   import { createStackNavigator } from '@react-navigation/stack';
   import { View, Text, FlatList, TouchableOpacity, StyleSheet, Alert } from 'react-native';
   import MapView, { Marker } from 'react-native-maps'; // For mock tracking

   const Stack = createStackNavigator();

   // Mock data for groceries
   const groceries = [
     { id: '1', name: 'Apples', price: 50, category: 'Fruits' },
     { id: '2', name: 'Milk', price: 30, category: 'Dairy' },
     { id: '3', name: 'Bread', price: 20, category: 'Bakery' },
     { id: '4', name: 'Rice', price: 100, category: 'Grains' },
   ];

   // Home Screen: Browse Groceries
   function HomeScreen({ navigation }) {
     const [cart, setCart] = useState([]);

     const addToCart = (item) => {
       setCart([...cart, item]);
       Alert.alert('Added to Cart', `${item.name} added!`);
     };

     return (
       <View style={styles.container}>
         <Text style={styles.title}>Grocery App - Like Zepto/Blinkit</Text>
         <FlatList
           data={groceries}
           keyExtractor={(item) => item.id}
           renderItem={({ item }) => (
             <View style={styles.item}>
               <Text>{item.name} - ₹{item.price}</Text>
               <TouchableOpacity style={styles.button} onPress={() => addToCart(item)}>
                 <Text style={styles.buttonText}>Add to Cart</Text>
               </TouchableOpacity>
             </View>
           )}
         />
         <TouchableOpacity style={styles.navButton} onPress={() => navigation.navigate('Cart', { cart })}>
           <Text style={styles.buttonText}>View Cart ({cart.length})</Text>
         </TouchableOpacity>
       </View>
     );
   }

   // Cart Screen
   function CartScreen({ route, navigation }) {
     const { cart } = route.params;

     const checkout = () => {
       Alert.alert('Order Placed!', 'Your groceries are on the way.');
       navigation.navigate('Tracking');
     };

     return (
       <View style={styles.container}>
         <Text style={styles.title}>Your Cart</Text>
         <FlatList
           data={cart}
           keyExtractor={(item, index) => index.toString()}
           renderItem={({ item }) => (
             <View style={styles.item}>
               <Text>{item.name} - ₹{item.price}</Text>
             </View>
           )}
         />
         <TouchableOpacity style={styles.navButton} onPress={checkout}>
           <Text style={styles.buttonText}>Checkout & Track Delivery</Text>
         </TouchableOpacity>
       </View>
     );
   }

   // Delivery Tracking Screen (Mock)
   function TrackingScreen() {
     return (
       <View style={styles.container}>
         <Text style={styles.title}>Delivery Tracking</Text>
         <Text>ETA: 15 minutes (like Zepto!)</Text>
         <MapView
           style={styles.map}
           initialRegion={{
             latitude: 28.6139, // Example: Delhi coordinates
             longitude: 77.2090,
             latitudeDelta: 0.01,
             longitudeDelta: 0.01,
           }}
         >
           <Marker coordinate={{ latitude: 28.6139, longitude: 77.2090 }} title="Your Location" />
           <Marker coordinate={{ latitude: 28.6150, longitude: 77.2100 }} title="Delivery Partner" />
         </MapView>
       </View>
     );
   }

   export default function App() {
     return (
       <NavigationContainer>
         <Stack.Navigator>
           <Stack.Screen name="Home" component={HomeScreen} />
           <Stack.Screen name="Cart" component={CartScreen} />
           <Stack.Screen name="Tracking" component={TrackingScreen} />
         </Stack.Navigator>
       </NavigationContainer>
     );
   }

   const styles = StyleSheet.create({
     container: { flex: 1, padding: 20, backgroundColor: '#fff' },
     title: { fontSize: 24, fontWeight: 'bold', marginBottom: 20 },
     item: { flexDirection: 'row', justifyContent: 'space-between', padding: 10, borderBottomWidth: 1 },
     button: { backgroundColor: '#007bff', padding: 10, borderRadius: 5 },
     navButton: { backgroundColor: '#28a745', padding: 15, borderRadius: 5, marginTop: 20 },
     buttonText: { color: '#fff', textAlign: 'center' },
     map: { flex: 1, marginTop: 20 },
   });
   
