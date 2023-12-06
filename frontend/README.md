import axios from 'axios';
import React, { useEffect, useState } from 'react';
import { Navbar, Nav, NavDropdown } from 'react-bootstrap';


const FNavbar = () => {
  const [navbar, setNavbar] = useState([]);
  const [isMobile, setIsMobile] = useState(window.innerWidth < 1025);

  useEffect(() => {
    axios.get(URL).then((response) => {
      setNavbar(response.data.categories);
    });
  }, []);

  window.addEventListener('resize', () => {
    setIsMobile(window.innerWidth < 1025);
  });

  return (
    <>
      {isMobile ? null : (
        <Navbar className='px-5 py-3 border'>
          <Nav className="mr-auto">
            {navbar.map((category) => (
              category.name === 'Vegetables' || category.name === 'Fruits' || category.name === 'Groceries' ? (
                <NavDropdown
                  key={category['category-id']}
                  title={category.name}
                  id="basic-nav-dropdown"
                  className='mx-3 text-black'
                >
                  {category.children.map((child) => (
                    <NavDropdown.Item
                      key={child['category-id']}
                      href={`/category/${child['category-id']}`}
                      className='text-black'
                    >
                      {child.name}
                    </NavDropdown.Item>
                  ))}
                </NavDropdown>
              ) : (
                <Nav.Link
                  key={category['category-id']}
                  href={`/category/${category['category-id']}`}
                  className='mx-3 text-black'
                >
                  {category.name}
                </Nav.Link>
              )
            ))}
          </Nav>
        </Navbar>
      )}
    </>
  );
};

export default FNavbar;
