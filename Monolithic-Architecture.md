import React, { useState } from 'react';
import { motion } from 'framer-motion';
import { Database, Users, Server, Layout, Layers, Cpu, ArrowRight, Info } from 'lucide-react';

const ArchitectureDemo = () => {
  const [selectedArch, setSelectedArch] = useState('monolithic');
  const [hoveredComponent, setHoveredComponent] = useState(null);
  
  const architectures = {
    monolithic: {
      title: "Monolithic Architecture",
      color: "bg-blue-500",
      description: "Single deployable unit containing all components",
      components: [
        { 
          icon: Users, 
          label: "UI Layer",
          note: "Handles all user interface components, rendering, and user interactions. Single interface for entire application."
        },
        { 
          icon: Cpu, 
          label: "Business Logic",
          note: "Contains all application logic, processing, and business rules. Tightly coupled with other layers."
        },
        { 
          icon: Database, 
          label: "Database",
          note: "Single database handling all data storage. Direct access from business layer."
        }
      ]
    },
    twoTier: {
      title: "Two-Tier Architecture",
      color: "bg-green-500",
      description: "Client-Server separation with distributed responsibilities",
      components: [
        { 
          icon: Users, 
          label: "Client Tier",
          note: "Contains UI and some business logic. Handles user interactions and basic data processing."
        },
        { 
          icon: Server, 
          label: "Server Tier",
          note: "Manages business logic and data access. Processes requests from client tier."
        },
        { 
          icon: Database, 
          label: "Database",
          note: "Centralized data storage accessed through server tier. Better security than monolithic."
        }
      ]
    },
    threeTier: {
      title: "Three-Tier Architecture",
      color: "bg-purple-500",
      description: "Separated presentation, business, and data layers",
      components: [
        { 
          icon: Users, 
          label: "Presentation Tier",
          note: "Pure UI layer focusing on user interface and experience. No business logic."
        },
        { 
          icon: Layers, 
          label: "Business Tier",
          note: "Isolated business logic layer. Processes requests from presentation tier and communicates with data tier."
        },
        { 
          icon: Server, 
          label: "Data Tier",
          note: "Handles data persistence and data access logic. Abstracted from business logic."
        },
        { 
          icon: Database, 
          label: "Database",
          note: "Secure data storage with no direct access from presentation tier."
        }
      ]
    },
    microservices: {
      title: "Microservices Architecture",
      color: "bg-red-500",
      description: "Distributed services with independent deployments",
      components: [
        { 
          icon: Users, 
          label: "Client Apps",
          note: "Multiple client applications accessing services through API Gateway."
        },
        { 
          icon: Layout, 
          label: "API Gateway",
          note: "Central entry point routing requests to appropriate services. Handles authentication and load balancing."
        },
        { 
          icon: Server, 
          label: "Authentication Service",
          note: "Independent service managing user authentication and authorization."
        },
        { 
          icon: Server, 
          label: "User Service",
          note: "Manages user profiles and user-related operations independently."
        },
        { 
          icon: Server, 
          label: "Order Service",
          note: "Handles order processing with its own database and business logic."
        },
        { 
          icon: Server, 
          label: "Payment Service",
          note: "Manages payment processing independently from other services."
        },
        { 
          icon: Database, 
          label: "Independent Databases",
          note: "Each service has its own database, ensuring data independence and service autonomy."
        }
      ]
    }
  };

  return (
    <div className="w-full max-w-6xl mx-auto p-6 bg-white rounded-xl shadow-lg">
      {/* Architecture Selector */}
      <div className="flex flex-wrap gap-4 mb-8">
        {Object.entries(architectures).map(([key, arch]) => (
          <button
            key={key}
            onClick={() => setSelectedArch(key)}
            className={`px-4 py-2 rounded-lg font-semibold transition-colors ${
              selectedArch === key 
                ? `${arch.color} text-white` 
                : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
            }`}
          >
            {arch.title}
          </button>
        ))}
      </div>

      {/* Architecture Visualization */}
      <motion.div
        key={selectedArch}
        initial={{ opacity: 0, y: 20 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.5 }}
        className="bg-gray-50 rounded-xl p-8"
      >
        <h2 className="text-2xl font-bold mb-4">{architectures[selectedArch].title}</h2>
        <p className="text-gray-600 mb-8">{architectures[selectedArch].description}</p>

        <div className="flex flex-col items-center space-y-4">
          {architectures[selectedArch].components.map((component, index) => (
            <div key={index} className="flex items-center w-full max-w-2xl">
              <motion.div
                initial={{ x: -50, opacity: 0 }}
                animate={{ x: 0, opacity: 1 }}
                transition={{ delay: index * 0.1 }}
                className={`relative flex items-center space-x-3 p-4 rounded-lg ${architectures[selectedArch].color} bg-opacity-10 w-full
                  hover:bg-opacity-20 transition-all cursor-help`}
                onMouseEnter={() => setHoveredComponent(index)}
                onMouseLeave={() => setHoveredComponent(null)}
              >
                <component.icon className={`w-6 h-6 ${architectures[selectedArch].color.replace('bg-', 'text-')}`} />
                <span className="font-medium">{component.label}</span>
                <Info className="w-4 h-4 text-gray-400" />
                
                {/* Tooltip */}
                {hoveredComponent === index && (
                  <div className="absolute left-0 top-full mt-2 p-3 bg-white rounded-lg shadow-lg z-10 w-64">
                    <p className="text-sm text-gray-600">{component.note}</p>
                  </div>
                )}
              </motion.div>
              {index < architectures[selectedArch].components.length - 1 && (
                <ArrowRight className="mx-4 text-gray-400" />
              )}
            </div>
          ))}
        </div>

        {/* Additional Information */}
        <div className="mt-8 p-4 bg-white rounded-lg shadow">
          <h3 className="font-semibold mb-2">Key Characteristics:</h3>
          <ul className="list-disc pl-5 space-y-2">
            {selectedArch === 'monolithic' && (
              <>
                <li>Single codebase and deployment unit</li>
                <li>Simplified development and testing process</li>
                <li>Tightly coupled components make changes challenging</li>
                <li>Vertical scaling only - entire application must be scaled</li>
              </>
            )}
            {selectedArch === 'twoTier' && (
              <>
                <li>Clear separation between client and server</li>
                <li>Better resource utilization than monolithic</li>
                <li>Simplified maintenance and updates</li>
                <li>Limited horizontal scaling capabilities</li>
              </>
            )}
            {selectedArch === 'threeTier' && (
              <>
                <li>Complete separation of presentation, business, and data concerns</li>
                <li>Enhanced security through layered access</li>
                <li>Independent scaling of each tier possible</li>
                <li>Better code reusability and maintenance</li>
              </>
            )}
            {selectedArch === 'microservices' && (
              <>
                <li>Independent deployment and scaling of services</li>
                <li>Complete service isolation reduces system-wide failures</li>
                <li>Freedom to use different technologies per service</li>
                <li>Complex distributed system management</li>
                <li>Perfect for large-scale applications and teams</li>
              </>
            )}
          </ul>
        </div>
      </motion.div>
    </div>
  );
};

export default ArchitectureDemo;
